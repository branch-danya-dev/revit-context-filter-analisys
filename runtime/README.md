# Runtime

This area owns **how ContextFilter keeps derived knowledge responsive, bounded and fresh during a live Revit session**.

## Canonical question

> How can the plugin avoid blocking Revit while also avoiding decisions based on stale context, stale indexes or obsolete requests?

## Main responsibilities

- multi-level caching of collected IDs, context tree, parameter index and filter result;
- invalidation on view / selection / document changes;
- incremental patching when a bounded change can be reconciled safely;
- full invalidation when change scope is too large or uncertain;
- debounced filter/model-change processing;
- request coalescing so obsolete pending work is replaced by newer intent;
- chunked context collection for larger element sets;
- session gating so background Idling work is not performed while the filter is inactive.

## Freshness before speed

```text
fast stale result
!= valid result
```

Optimization is allowed only while preserving the semantic relationship:

```text
current Revit evidence
→ current context
→ current catalog/index
→ current filter result
```

## Current implementation evidence

The implementation analysis records concrete tuning thresholds for debounce, parallel evaluation, chunked collection, incremental patching, live highlighting and warnings for large entire-document scopes.

Those numbers are implementation policy and may evolve. The canonical runtime requirement is that large contexts remain usable without changing filter semantics or silently trusting stale derived state.

## Request coalescing

Repeated refresh/index/highlight requests may be collapsed so that only the latest relevant intent remains pending.

```text
old pending intent
+ newer equivalent-type intent
→ discard obsolete work
→ execute latest intent
```

## Does not own

- context semantics → [`../context/`](../context/);
- filter logic → [`../filtering/`](../filtering/);
- Revit main-thread authority → [`../revit-boundary/`](../revit-boundary/).
