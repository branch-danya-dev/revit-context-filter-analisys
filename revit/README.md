# Revit

`ContextFilter.Revit` — host-specific слой и точка входа add-in.

Он владеет тем, **как Domain/Application intent реализуется через Autodesk Revit API**.

Основные области:

- [`external-event/`](external-event/) — безопасный мост WPF → Revit main thread;
- [`collection/`](collection/) — `FilteredElementCollector`, tree records и chunked collection;
- [`actions/`](actions/) — native selection / visibility / view-filter operations;
- [`lifecycle/`](lifecycle/) — document/view/selection events и shutdown;
- [`parameters/`](parameters/) — Revit Parameter ↔ Domain ParameterKey/Value;
- [`transactions/`](transactions/) — host write boundaries.

Revit является authority для host state; plugin caches и snapshots — derived representations.
