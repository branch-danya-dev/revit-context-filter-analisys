# Component Model

ContextFilter реализован как Clean Architecture solution из пяти основных проектов.

## Domain

`ContextFilter.Domain` хранит независимый смысл системы:

- filter tree;
- `FilterDefinition`;
- operators;
- `ParameterKey`;
- `ElementSnapshot`;
- result models;
- preset models;
- domain interfaces.

Domain не зависит от Revit, WPF или persistence implementation.

## Application

`ContextFilter.Application` владеет use cases и orchestration:

- collect context;
- build Category → Family → Type projection;
- build parameter indexes / values;
- compile quick filter into `FilterDefinition`;
- evaluate filter;
- calculate selection / visibility sets;
- build, save, delete and list presets;
- analyze native-filter compatibility;
- cache / debounce coordination.

Application зависит от Domain.

## Infrastructure

`ContextFilter.Infrastructure` реализует технические адаптеры persistence:

- `settings.json`;
- `presets.json`;
- `recent.json`;
- atomic writes;
- schema migration;
- logging / DI extensions.

Infrastructure поддерживает Application/Domain, но не определяет filter semantics.

## UI

`ContextFilter.UI` отвечает за WPF presentation и interaction:

- Views / ViewModels;
- трёхзонный рабочий интерфейс;
- UI state;
- user feedback;
- команды и interaction behavior.

UI не является владельцем Revit document state.

## Revit

`ContextFilter.Revit` адаптирует систему к host application:

- `IExternalApplication`;
- ribbon / pane lifecycle;
- `ExternalEvent`;
- collection из Revit document;
- conversion в Domain representations;
- selection / visibility / native filter actions;
- Revit events;
- transactions;
- shutdown / resource release.

## Dependency direction

```text
Revit ───────→ UI
  │            │
  │            └────→ Application ─────→ Domain
  │                         ↑
  └────→ Infrastructure ────┘
```

Физическая зависимость слоя и semantic authority — разные вещи. Например, Revit project зависит от Domain types, но сам Revit document остаётся authority для host state.
