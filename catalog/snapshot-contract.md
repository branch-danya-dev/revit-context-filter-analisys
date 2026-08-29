# Element Snapshot Contract

## What this document describes

This document defines the semantic role of the in-memory **element snapshot** used by ContextFilter.

It answers:

> What filterable knowledge about one Revit element may leave the direct Revit API boundary and be evaluated by the client-side filter engine?

## What it intentionally does not describe

It does not define:

- Revit API extraction mechanics;
- filter operator semantics;
- cache thresholds or parallel execution;
- native Revit filter conversion.

Those belong to [`../revit-boundary/`](../revit-boundary/), [`../filtering/`](../filtering/) and [`../runtime/`](../runtime/).

## Implementation evidence

The delivered implementation contains an `ElementSnapshot` with:

```text
ElementId
UniqueId
CategoryIdentity
FamilyTypeIdentity
Parameters: ParameterKey → ParameterValue
ParameterDisplayNames: ParameterKey → string
```

Filtering is performed client-side on these in-memory snapshots rather than by repeatedly executing every semantic condition through direct Revit API access.

The implementation also supports **light snapshots** and later lazy loading of deeper parameter data.

## Semantic model

```text
REVIT ELEMENT
     ↓ read through host boundary
SOURCE FACTS
     ↓ project
ELEMENT SNAPSHOT
     ↓
CLIENT-SIDE FILTER EVALUATION
```

The snapshot is therefore:

> **a derived, filter-oriented representation of a source Revit element for one valid context state.**

It is not a second authoritative copy of the element.

## Identity retained in the snapshot

The snapshot preserves both numeric `ElementId` and Revit `UniqueId` in the current implementation.

This document does not redefine Revit's identity guarantees. It only records that the client-side representation retains source identifiers so downstream results can be related back to host elements.

```text
snapshot identity evidence
→ relate derived result to source element
```

## Classification retained in the snapshot

The snapshot includes category and family/type identities independently from the deeper parameter map.

This allows the system to build useful catalog knowledge before all parameter values have been materialized.

```text
LIGHT SNAPSHOT
Element identity
+ Category
+ Family / Type

ENRICHED SNAPSHOT
light data
+ requested parameter knowledge
```

## Lazy enrichment

Lazy loading is an implementation strategy, but it creates a semantic requirement:

> **A snapshot can be valid while still not containing every possible parameter value.**

Therefore:

```text
not loaded
!= parameter missing
```

This distinction is critical.

`MISSING` is a semantic statement about the source element/parameter relationship. `NOT LOADED` is a statement about the current completeness of the derived representation.

The filter layer must not infer `NotExists` merely because a parameter has not yet been materialized into a light snapshot.

## Snapshot completeness

Conceptually, snapshot knowledge has at least two dimensions:

```text
SOURCE VALIDITY
Is this snapshot still derived from the current Revit/context state?

ENRICHMENT COMPLETENESS
Does this snapshot contain the parameter knowledge required for the current operation?
```

A snapshot may be:

```text
current + light
current + sufficiently enriched
stale + light\stale + enriched
```

Only current snapshots with enough materialized knowledge for the requested operation should participate in authoritative filter evaluation.

## Snapshot ownership

- Revit owns the source element and native parameter facts.
- [`context/`](../context/) owns whether the source candidate universe is current.
- **Catalog owns the snapshot representation used by filtering.**
- [`filtering/`](../filtering/) owns what conditions mean when evaluated over that representation.

## Parameter maps

The implementation separates:

```text
ParameterKey → ParameterValue
ParameterKey → DisplayName
```

This separation reinforces that:

```text
value storage
!= presentation metadata

ParameterKey
!= display label
```

See [`parameter-identity.md`](parameter-identity.md).

## Synthetic values

Synthetic filterable properties such as Category, Family, TypeName, ElementId, UniqueId, Workset and Level are produced during snapshot construction.

This means one snapshot may expose a unified filterable surface composed of facts with different physical origins:

```text
native Revit parameter
or
source element metadata / derived synthetic property
        ↓
normalized filterable snapshot knowledge
```

Unified filtering does not imply identical source representation.

## Freshness propagation

A snapshot is derived from both a source element and a context state.

When source/context evidence changes beyond what can be safely patched:

```text
source evidence changed
→ old snapshot knowledge may no longer be true
→ invalidate / rebuild affected snapshot knowledge
→ rebuild dependent parameter indexes / values as needed
```

Catalog does not decide the runtime patch threshold. It owns the correctness requirement that stale snapshot knowledge must not be presented as current.

## Failure semantics

If deeper parameter enrichment fails, the system must not silently reinterpret the failure as a valid empty/missing parameter state.

Conceptually:

```text
failed to read / enrich
!= source parameter missing
!= source parameter empty
```

The exact user-visible failure behavior is a cross-boundary concern to be detailed later in failure models.

## Invariants

1. Snapshot data is derived; Revit remains source authority.
2. A snapshot belongs to the context/source state from which it was produced.
3. Light snapshot does not mean missing parameters.
4. Lazy enrichment may add knowledge without changing the source element identity.
5. Display names are separate from canonical parameter identity.
6. Synthetic and native facts may share one filterable representation without sharing one physical origin.
7. Stale snapshots must not be used as if they were current source truth.

## Typical analytical mistakes

### Snapshot = cached Revit element

A snapshot is not the host object and does not inherit host authority.

### Parameter not in dictionary = NotExists

That is invalid when lazy materialization exists. Absence from a partially enriched representation is not automatically semantic absence.

### Enrichment = new context

Adding requested parameter data to the same current candidate universe enriches Catalog knowledge; it does not automatically redefine Context scope.

## Check

For any snapshot-based operation we should be able to answer:

- which source element this snapshot represents;
- which context/source state it belongs to;
- which facts are already materialized;
- whether required parameter knowledge is loaded;
- whether an absent value is truly missing or merely not loaded;
- how the derived result will be related back to Revit.
