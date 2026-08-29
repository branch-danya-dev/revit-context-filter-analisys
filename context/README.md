# Context

This area owns **which Revit elements belong to one filtering session and when that derived context must be considered stale**.

## Canonical question

> What is the current search universe for this filter, and may the plugin still trust its derived snapshot?

## Supported scopes

```text
ActiveView
EntireDocument
CurrentSelection
```

These scopes are semantically different. They must never be merged implicitly.

### Active view

The candidate set is derived from elements available in the current view context.

### Entire document

The candidate set is derived from the full document and may require a warning or progressive collection for large models.

### Current selection

The candidate set is bounded by the explicit Revit selection captured for the session. Selection changes therefore affect context identity.

## Context identity

A current implementation cache key includes evidence such as:

```text
Document
+ View
+ Scope
+ Selection
+ Model change version
```

These fields are implementation representation of a semantic rule:

> **A cached context is valid only for the source state from which it was derived.**

## Freshness

View changes, selection changes and document changes can reopen the context.

Small document changes may be patched incrementally. Larger or uncertain changes invalidate deeper derived knowledge and require recollection.

```text
Revit evidence changes
→ context may become stale
→ patch safely OR invalidate
→ rebuild before authoritative use
```

## Ownership

- Revit owns source element existence and current host state.
- ContextFilter owns the derivation of one bounded `CollectedContext`.
- Cache storage does not become authority for Revit facts.

## Does not own

- Category/Family/Type representation → [`../catalog/`](../catalog/);
- filter evaluation → [`../filtering/`](../filtering/);
- performance strategy → [`../runtime/`](../runtime/).
