# Actions

`actions/` описывает реальные host-side effects, которые Revit layer выполняет после Application calculations.

## Подтверждённые services

- `SelectionActionService` — `UIDocument.Selection.SetElementIds`, optional zoom to selection;
- `VisibilityActionService` — temporary Hide/Isolate;
- `NativeFilterActionService` — создание `ParameterFilterElement` и привязка к виду;
- `FilterDefinitionToElementFilterConverter` — Domain filter → Revit `ElementFilter` subset;
- `RevitParameterResolver` — `ParameterKey` → Revit parameter representation.

## Общая граница

```text
Application target set / filter intent
↓
IRevitGateway request
↓
Revit action service
↓
Revit API
↓
host state changed or failure returned
```

## Документы

- [`selection.md`](selection.md)
- [`visibility.md`](visibility.md)
- [`native-filter.md`](native-filter.md)

## Ключевое различие

```text
FilterResult
!= action target set
!= successful Revit side effect
```

Каждый переход имеет собственную failure boundary.
