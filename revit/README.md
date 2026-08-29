# Revit

`ContextFilter.Revit` — host-adapter слой ContextFilter. Он связывает Domain/Application/UI с Autodesk Revit 2025 и владеет всеми ограничениями, которые появляются именно потому, что система выполняется внутри Revit.

## Ответственность

Revit layer отвечает за:

- вход в Revit через `IExternalApplication` и ribbon command;
- composition root приложения;
- безопасный доступ к Revit API через `ExternalEvent`;
- очередь и dispatch host requests;
- сбор элементов из `Document` / `View` / текущего selection;
- преобразование Revit elements/parameters в Domain representations;
- реальные selection / visibility / native-filter actions;
- соблюдение transaction requirements;
- реакцию на Revit lifecycle events;
- session-bound `Idling` work;
- host-specific shutdown и hotkey integration.

## Граница authority

```text
Autodesk Revit
→ authority for Document / Element / View / Selection

ContextFilter Domain/Application
→ authority for filter meaning and application workflow

ContextFilter.Revit
→ adapter between those authorities
```

Revit layer не определяет семантику `FilterDefinition`, `ParameterKey`, preset или action intent. Он должен корректно реализовать уже определённый смысл в рамках host API.

## Основной pipeline

```text
UI / Application
      ↓
IRevitGateway
      ↓
RevitExternalEventDispatcher
      ↓
RevitRequestQueue
      ↓
ExternalEvent.Raise()
      ↓
ContextFilterExternalEventHandler
      ↓
RevitRequestDispatcher
      ↓
Revit service
      ↓
Revit API
      ↓
response
```

Revit API не thread-safe, поэтому этот pipeline является системной границей, а не просто технической обёрткой.

## Структура

```text
revit/
├─ external-event/   → main-thread request pipeline
├─ collection/       → host-side context collection
├─ parameters/       → Revit ↔ Domain parameter translation
├─ actions/          → реальные host-side effects
├─ transactions/     → write execution boundary
├─ lifecycle/        → startup, events, session, shutdown
└─ diagrams/         → host integration view
```

Дополнительно [`composition-root.md`](composition-root.md) описывает `AddinHost` и сборку runtime dependencies.

## Ключевые различия

```text
Application port
!= Revit adapter

Domain action intent
!= Revit API call

valid filter
!= native Revit filter

UI async Task
!= permission to access Revit API

successful filter evaluation
!= successful host mutation

Document lifecycle
!= plugin pane lifecycle
```
