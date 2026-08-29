# Context

This area owns **which Revit elements belong to one filtering session, which source state that candidate set came from, and when the derived context must no longer be trusted**.

## Canonical question

> What is the current search universe for this filter, what source state produced it, and is that context still valid?

Context is not the filter and not the visible Category → Family → Type tree. It is the bounded source universe over which those downstream models are built.

```text
REVIT SOURCE STATE
       ↓
SCOPE DECISION
       ↓
BOUNDED CANDIDATE SET
       ↓
CONTEXT IDENTITY + FRESHNESS
       ↓
CATALOG / FILTERING
```

## Canonical knowledge in this area

- [`scope-model.md`](scope-model.md) — semantic meaning of `ActiveView`, `EntireDocument` and `CurrentSelection`;
- [`context-contract.md`](context-contract.md) — what Context provides downstream and what remains owned elsewhere;
- [`freshness-lifecycle.md`](freshness-lifecycle.md) — when context remains current, becomes stale, is patched, invalidated or rebuilt.

## Core invariants

1. **Exactly one scope defines one context.** Scope boundaries are never merged implicitly.
2. **Revit remains source authority.** The plugin owns a derivation, not source element truth.
3. **A candidate set is meaningful only with provenance.** Element IDs without document/scope/source identity are insufficient context.
4. **CurrentSelection is selection-bound.** A changed selection can define a different context even in the same document and view.
5. **ActiveView is view-bound.** A view change reopens the candidate-set claim.
6. **EntireDocument is document-bound.** It does not become view-scoped merely because the UI currently displays one view.
7. **Freshness is part of correctness.** A previously correct derived context may become wrong after source changes.
8. **Patchability is not authority.** Incremental repair is allowed only when the implementation can preserve the same semantic context with confidence.
9. **Physical data co-location does not merge responsibilities.** An implementation object may carry context IDs and catalog projections together while their canonical meanings remain separately owned.

## Supported scopes

```text
ActiveView
EntireDocument
CurrentSelection
```

The current implementation exposes these as explicit collection scopes. `EntireDocument` may require warning/progressive collection for large models, while selection changes invalidate `CurrentSelection` context.

See [`scope-model.md`](scope-model.md).

## Context identity

The implementation cache identity contains evidence such as:

```text
DocumentKey
+ ViewId
+ Scope
+ SelectionHash
+ ChangeVersion
```

Those fields are implementation representation of a broader semantic rule:

> **A cached context is reusable only while the source identity and source state relevant to that scope remain compatible with the context that was derived.**

The exact cache key is not the canonical business object. The canonical claim is the provenance/freshness relationship.

## Freshness

Source events can reopen the context claim:

```text
view changes
selection changes
document changes
document closes / switches
        ↓
context may no longer describe current Revit state
        ↓
keep / patch / invalidate / rebuild
```

The implementation may incrementally patch smaller known document changes and invalidate more broadly when safe repair is not justified. Thresholds and scheduling are runtime mechanics; the Context responsibility owns the semantic requirement that stale derived knowledge must not be treated as current.

See [`freshness-lifecycle.md`](freshness-lifecycle.md).

## Ownership

| Knowledge | Owner |
|---|---|
| Source element existence and current Revit document/view/selection state | **Revit** |
| Scope decision for the current filtering session | **ContextFilter / Context** |
| Bounded candidate element set and its provenance | **ContextFilter / Context** |
| Category / Family / Type representation of those candidates | [`catalog/`](../catalog/) |
| Filter meaning and matched set | [`filtering/`](../filtering/) |
| Cache scheduling, chunking, coalescing and performance thresholds | [`runtime/`](../runtime/) |

## Does not own

- Category/Family/Type hierarchy and parameter/value projection → [`../catalog/`](../catalog/);
- filter definition or evaluation → [`../filtering/`](../filtering/);
- user action semantics → [`../actions/`](../actions/);
- host API execution rules → [`../revit-boundary/`](../revit-boundary/);
- performance thresholds and work scheduling → [`../runtime/`](../runtime/).

## Key distinction

```text
source Revit state
!= collected context

collected context
!= catalog projection

cached context
!= source authority

same document
!= same context

same element IDs
!= necessarily same provenance
```
