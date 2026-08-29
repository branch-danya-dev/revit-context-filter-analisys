# Collection

`collection/` описывает чтение текущего Revit source state и преобразование его в данные, пригодные для Application/Domain.

## Подтверждённые компоненты

- `RevitElementCollector` — `FilteredElementCollector` по выбранному scope;
- `RevitElementTreeReader` — Category / Family / Type → `ElementTreeRecord`;
- `SupportedElementRules` — исключение `View`, `ElementType` и internal categories;
- `ChunkedCollectionSession` — порционный сбор на `Idling`;
- `ContextCollectionService` — координация cache, patch и sync/chunked collection;
- `RevitContextState` — document/view/selection tracking.

## Граница

```text
Revit Document / View / Selection
↓
collection adapter
↓
source records / snapshots
↓
Application projection
```

Revit layer отвечает за корректное чтение host state. Он не определяет смысл `CollectionScope` — этот enum принадлежит Domain.

## Документы

- [`scope-collection.md`](scope-collection.md)
- [`chunked-collection.md`](chunked-collection.md)
- [`context-state.md`](context-state.md)

## Инвариант

> Derived collection data должно оставаться связано с тем Revit document/view/selection state, из которого оно было получено.
