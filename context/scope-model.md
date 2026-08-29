# Scope Model

## What this document describes

This document defines **how one filtering session chooses its search universe**.

It intentionally does not define:

- how categories or parameters are presented;
- how a filter is evaluated;
- how actions are applied to matched elements;
- performance thresholds used to collect the scope.

Canonical knowledge here is the semantic meaning of the three supported scopes and the provenance required to distinguish them.

## Problem

A context filter is only meaningful if the system can answer:

> **Which Revit elements are eligible to participate in this filtering session before any filter condition is evaluated?**

Without an explicit scope, the same filter definition can produce different results depending on current view, document or selection state.

## Scope is a first-class decision

```text
FILTER SESSION
      ↓
SCOPE
      ↓
CANDIDATE SET
      ↓
CATALOG
      ↓
FILTER EVALUATION
```

The filter does not decide its own universe. Context does.

## ActiveView

### Meaning

`ActiveView` means that candidates are collected from the current Revit view context.

```text
current document
+ active view
→ eligible source elements for that view
→ candidate set
```

### Consequence

A view change can invalidate the claim that the current candidate set still represents `ActiveView`.

The same filter definition applied after a view change may legitimately produce a different result because the source universe changed.

### Canonical ownership

- Revit owns which document and view are active and which elements are available in that host context.
- ContextFilter owns the derived candidate set for the selected `ActiveView` scope.

## EntireDocument

### Meaning

`EntireDocument` means that the search universe is bounded by the current Revit document rather than the active view.

```text
current document
→ supported source elements in document
→ candidate set
```

### Consequence

The active view is not the semantic boundary of the candidate set.

A view change alone does not turn an `EntireDocument` context into an `ActiveView` context. However, document changes can still make the derived candidate set stale.

### Large contexts

Implementation evidence shows that the full-document scope may warn for larger models and may use progressive/chunked collection. Those are execution strategies, not a different scope meaning.

```text
EntireDocument
!= "whatever can be collected quickly"
```

The semantic scope stays the whole supported document even if collection is incremental.

## CurrentSelection

### Meaning

`CurrentSelection` means that the candidate set is bounded by the explicit Revit selection associated with the current session.

```text
current document
+ selected element identities
→ candidate set
```

### Consequence

Selection is part of context identity.

A changed Revit selection can therefore mean a changed context even when document and active view remain the same.

Implementation evidence reflects this with a `SelectionHash` in context/cache identity and explicit invalidation of `CurrentSelection` when selection changes.

## Scope comparison

| Question | ActiveView | EntireDocument | CurrentSelection |
|---|---|---|---|
| Primary boundary | active Revit view | current Revit document | explicit Revit selection |
| View change relevant? | yes | not by itself to scope meaning | only if host selection/context changes as a result |
| Selection change relevant? | not to scope identity itself | not to scope identity itself | yes |
| Document change relevant? | yes | yes | yes |
| Candidate count potentially large? | yes | highest risk | bounded by selection |
| May require progressive collection? | implementation-dependent | commonly yes for larger models | usually smaller, but not semantically guaranteed |

## Scope identity

The implementation represents context identity using information such as:

```text
DocumentKey
ViewId
Scope
SelectionHash
ChangeVersion
```

Not every field has equal semantic importance for every scope.

For example:

```text
ActiveView
→ Document + View + relevant source version

EntireDocument
→ Document + relevant source version

CurrentSelection
→ Document + Selection + relevant source version
```

The implementation may keep a uniform cache key for convenience. SSAD separates that physical representation from the semantic identity rules.

## Invalid combinations

The system must not silently reinterpret one scope as another.

Examples:

```text
CurrentSelection requested
+ selection changes
→ do not silently keep old selection as "current"

EntireDocument requested
+ active view changes
→ do not silently narrow to new active view

ActiveView requested
+ view changes
→ do not silently present old view context as current
```

If a requested scope cannot currently be established, interaction may expose it as unavailable or require recollection. It should not manufacture a different scope without an explicit user/system decision.

## Result

A valid scope decision produces a bounded candidate universe with enough provenance to answer:

```text
which document?
which scope?
which view if relevant?
which selection if relevant?
which source version / freshness evidence?
```

Only after that may downstream responsibilities build catalogs or evaluate filters.

## Typical errors

- treating active view as an implicit universal default even after another scope was selected;
- keeping `CurrentSelection` results after selection has changed;
- confusing collection performance strategy with scope semantics;
- using cached element IDs without validating their provenance;
- deriving filter results before the candidate universe is stable enough to trust.

## Check

A scope model is coherent if, for any filter result, the system can explain **which candidate universe was searched and why that universe was still valid at evaluation time**.
