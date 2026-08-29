# Context Contract

## What this document describes

This document defines **what the Context responsibility guarantees to downstream parts of ContextFilter**.

It intentionally does not prescribe a C# interface or one specific DTO shape.

The canonical knowledge is semantic:

> Context provides a bounded candidate universe together with enough provenance and freshness evidence to decide whether downstream derived knowledge may still be used.

## Why a contract is needed

Implementation evidence physically groups several kinds of data inside `CollectedContext`, including:

- scope;
- element IDs;
- a cache key;
- context tree roots;
- tree records.

That physical object is useful implementation evidence, but it must not collapse multiple responsibilities into one conceptual owner.

```text
one implementation object
can carry
multiple semantic projections
```

Therefore:

```text
CollectedContext DTO
!= one semantic responsibility
```

## Context-owned output

At the semantic level, Context owns:

```text
CandidateContext
├─ source document identity
├─ selected scope
├─ scope-specific provenance
├─ candidate element identities
└─ freshness / source-version evidence
```

This is the minimum meaning required downstream.

## What is not context-owned

### Catalog projection

The following are derived from the candidate set but owned by [`../catalog/`](../catalog/):

```text
Category
→ Family
→ Type

available parameters
parameter display groups
available parameter values
```

Even if these values travel in the same implementation response, their semantic owner is Catalog.

### Filter result

The matched element set is owned by [`../filtering/`](../filtering/), because it depends on both:

```text
candidate context
+
filter definition
```

Context does not own the statement "these elements match the filter".

### Action result

Selection, visibility and native-filter outcomes are owned by [`../actions/`](../actions/) and realized through [`../revit-boundary/`](../revit-boundary/).

## Downstream precondition

Catalog and Filtering may use a context only when its freshness contract is satisfied.

```text
context exists
+ provenance still matches current source state
+ context not invalidated
→ downstream derivation allowed
```

If freshness is uncertain:

```text
uncertain source compatibility
→ context not authoritative enough for reuse
→ patch safely OR invalidate/rebuild
```

## Context result is derived knowledge

Revit remains authority for source facts.

ContextFilter may know:

> "At source version V, under scope S, these element identities formed the candidate set."

It does **not** gain authority to claim:

> "These elements still exist and still belong to the current source context regardless of later Revit changes."

This gives the central trust rule:

```text
Context evidence
is conditionally trusted
only while its provenance remains valid
```

## Source identity and element identity

A list of element IDs alone is insufficient as a reusable context.

```text
[101, 102, 103]
```

without provenance does not answer:

- from which Revit document did these IDs come?
- from which scope?
- from which view or selection when relevant?
- before or after which model changes?

Therefore:

> **Element identity without source-context identity is not a complete Context contract.**

## Physical representation vs semantic meaning

Implementation evidence uses a uniform `ContextCacheKey` with values such as `DocumentKey`, `ViewId`, `Scope`, `SelectionHash` and `ChangeVersion`.

SSAD does not require every future implementation to preserve that exact type or exact field layout.

The invariant is:

```text
physical cache identity may change

but

semantic provenance required to validate context
must remain expressible
```

## Consumers

| Consumer | What it receives from Context | What it adds |
|---|---|---|
| Catalog | bounded candidate set + provenance | category/family/type and parameter projection |
| Filtering | valid candidate universe, directly or via snapshots derived from it | filter evaluation and matched set |
| Runtime | identity/freshness dependencies | caching, scheduling, patch/invalidation mechanics |
| Interaction | scope availability/current-context status | user-facing session state |

## Failure contract

Context must not present a stale or source-mismatched candidate set as current merely because cached data is available.

When a context cannot be safely established or restored, downstream behavior must distinguish:

```text
no current context
!= empty valid context

stale context
!= current context with zero matches
```

This distinction prevents infrastructure state from being mistaken for a valid user result.

## Check

The contract is correct if downstream components can answer all three questions before using context-derived knowledge:

```text
WHAT is the candidate universe?
WHERE did it come from?
WHY may we still trust it now?
```
