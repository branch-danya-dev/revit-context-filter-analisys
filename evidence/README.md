# Evidence

This area owns **where claims about ContextFilter came from and how requirement evidence is separated from implementation evidence and current synthesis**.

## Canonical question

> Which facts are directly supported by project artifacts, which describe the implemented system, and which are analytical conclusions built from both?

## Evidence classes

### Requirement evidence

Customer-provided requirement drafts describe the problem, expected workflow, examples and requested behaviors.

They are historical evidence. Empty template fields are not silently treated as requirements.

### Implementation evidence

A later source-code-derived project analysis describes the implemented architecture, filter model, Revit integration, persistence, runtime behavior, testing and known trade-offs.

Implementation is evidence of what the delivered system actually does, but code-layer names do not define SSAD knowledge ownership.

### SSAD synthesis

The current repository assigns supported claims to stable system responsibilities and resolves terminology into one canonical model.

```text
requirements
+
implementation
→ compare / reconcile
→ responsibility owners
→ current system model
```

## Source handling

Raw customer documents are not copied into this public repository. Public evidence notes are sanitized summaries of the knowledge needed to explain the case.

An unrelated DWG-export requirement artifact supplied alongside the ContextFilter materials is intentionally excluded from the case because the available evidence does not establish it as part of ContextFilter's system boundary.

## Current evidence map

| Evidence | What it supports |
|---|---|
| Initial ContextFilter requirement draft | original manual problem; quick selection/hide/isolate; category/family/type and parameter filtering; correctness concern around type matching |
| Refined ContextFilter requirement draft | preselection scope; dynamic highlighting; presets/templates; three-zone interaction; AND/OR; exclude/inverse actions; Revit 2025 |
| Source-code-derived implementation analysis | current filter model; scopes; parameter identity; snapshots; actions; persistence; runtime; ExternalEvent boundary; tests and trade-offs |

See [`requirements-evolution.md`](requirements-evolution.md).
