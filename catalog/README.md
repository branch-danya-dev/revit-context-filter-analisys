# Catalog

This area owns **how the current source context is projected into stable filterable knowledge: classification, element snapshots, parameter identity and parameter values**.

## Canonical question

> What filterable facts can the system expose about the current context, and how are those facts identified without confusing UI labels, Revit representation and semantic identity?

## Canonical route

```text
CURRENT CONTEXT
      ↓
ELEMENT CLASSIFICATION
      ↓
Category → Family → Type
      ↓
ELEMENT SNAPSHOTS
      ↓
PARAMETER IDENTITIES
      ↓
PARAMETER VALUES
      ↓
FILTERABLE CATALOG
```

Detailed models:

- [`projection-model.md`](projection-model.md) — Category → Family → Type as a derived navigation projection;
- [`snapshot-contract.md`](snapshot-contract.md) — what an `ElementSnapshot` means and what lazy enrichment may change;
- [`parameter-identity.md`](parameter-identity.md) — stable identity across built-in/shared/project/synthetic and instance/type sources;
- [`value-semantics.md`](value-semantics.md) — absent, empty and concrete values, typed representation and value discovery.

## Core distinctions

```text
source Revit element
!= catalog projection

Category / Family / Type tree
!= source ownership

parameter display name
!= parameter identity

instance parameter
!= type parameter

synthetic property
!= native Revit parameter

parameter absent
!= parameter exists but empty
!= parameter has concrete value

light snapshot
!= fully enriched snapshot

catalog knowledge
!= filter intent
```

## Context tree

The current implementation exposes a three-level navigation projection:

```text
Category
  └─ Family
       └─ Type
```

The implementation builds this projection from `ElementTreeRecord` data and associates tree nodes with source element IDs. The tree is therefore **derived navigation knowledge over the current context**; it is not a second model of Revit ownership.

## Parameter identity

The implementation represents parameter identity conceptually as:

```text
ParameterIdentityKind
+ IdentityValue
+ ParameterSource
```

with current evidence distinguishing at least built-in, shared, project and synthetic identities, and instance, type and synthetic sources.

A separate mapping stores parameter display names. This is an important design signal:

> **A label shown to the user may describe a parameter, but it does not canonically identify that parameter.**

See [`parameter-identity.md`](parameter-identity.md).

## Synthetic filterable properties

Current implementation evidence exposes synthetic keys for:

- `Category`;
- `Family`;
- `TypeName`;
- `ElementId`;
- `UniqueId`;
- `Workset`;
- `Level`.

These values are computed while building snapshots rather than read as ordinary Revit parameters.

This gives them the same **filterable role** without falsely claiming the same **source representation**.

## Snapshot model

Client-side filtering operates on in-memory `ElementSnapshot` structures rather than repeatedly evaluating every condition through direct Revit API calls.

A snapshot contains element identity, category/family/type classification and a map of `ParameterKey → ParameterValue`. Parameter data may be loaded lazily: lightweight classification can exist before deeper parameter materialization.

See [`snapshot-contract.md`](snapshot-contract.md).

## Values

The catalog distinguishes:

```text
MISSING
EMPTY
CONCRETE VALUE
```

Implementation evidence confirms that the quick-filter layer maps the special missing/empty cases to different semantic operators (`NotExists` and `IsEmpty`). Therefore collapsing them in Catalog would already destroy information required for correct filtering.

See [`value-semantics.md`](value-semantics.md).

## Freshness dependency

Catalog does not decide whether its source context is current.

```text
Context becomes stale
→ catalog derived from that context cannot be treated as current
→ rebuild / re-enrich from current context before authoritative filtering
```

Context freshness remains canonical in [`../context/freshness-lifecycle.md`](../context/freshness-lifecycle.md).

## Ownership

| Knowledge | Canonical owner |
|---|---|
| Source elements and native parameter facts | Revit |
| Candidate element universe and its freshness | [`context/`](../context/) |
| Category / Family / Type projection | **Catalog** |
| Snapshot representation used by client-side filtering | **Catalog** |
| Semantic parameter identity used by filtering | **Catalog** |
| Filterable value representation and missing/empty distinction | **Catalog** |
| Meaning of operators and logical composition | [`filtering/`](../filtering/) |
| Revit-native parameter resolution | [`revit-boundary/`](../revit-boundary/) |
| Cache/index performance strategy | [`runtime/`](../runtime/) |

## Does not own

Catalog intentionally does **not** decide:

- which elements enter the session → [`../context/`](../context/);
- what `Equals`, `Contains`, `Between`, `Exists`, `IsEmpty`, AND or OR mean → [`../filtering/`](../filtering/);
- whether a semantic parameter/filter can be realized as a native Revit filter → [`../revit-boundary/`](../revit-boundary/);
- cache thresholds, parallelization or request coalescing → [`../runtime/`](../runtime/).

## Canonical rule

> **Catalog makes Revit facts filterable without pretending that their display representation, storage representation and semantic identity are the same thing.**
