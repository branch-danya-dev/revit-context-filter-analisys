# Действия

`actions/` описывает реальные изменения состояния Revit после расчётов Application.

Подтверждены:

- `SelectionActionService` — изменение `UIDocument.Selection`, дополнительный переход к выделению;
- `VisibilityActionService` — временное скрытие и изоляция;
- `NativeFilterActionService` — создание `ParameterFilterElement` и привязка к виду;
- `FilterDefinitionToElementFilterConverter` — преобразование поддерживаемой части Domain-фильтра в `ElementFilter`;
- `RevitParameterResolver` — разрешение `ParameterKey` в представление Revit.

```text
целевой набор / условия Application
→ запрос IRevitGateway
→ сервис действия Revit
→ Revit API
→ состояние изменено или возвращена ошибка
```

- [`selection.md`](selection.md)
- [`visibility.md`](visibility.md)
- [`native-filter.md`](native-filter.md)

```text
FilterResult
!= целевой набор действия
!= успешно выполненное действие Revit
```

Каждый переход имеет собственную границу ошибки.
