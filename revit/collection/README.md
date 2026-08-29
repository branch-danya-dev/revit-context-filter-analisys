# Сбор данных

`collection/` описывает чтение текущего исходного состояния Revit и преобразование его в данные для Application/Domain.

Подтверждены:

- `RevitElementCollector` — `FilteredElementCollector` по выбранной области;
- `RevitElementTreeReader` — Category / Family / Type → `ElementTreeRecord`;
- `SupportedElementRules` — исключение `View`, `ElementType`, внутренних категорий;
- `ChunkedCollectionSession` — порционный сбор на `Idling`;
- `ContextCollectionService` — координация кэша, локального обновления и обычного/порционного сбора;
- `RevitContextState` — отслеживание документа, вида и выделения.

```text
Revit Document / View / Selection
→ адаптер сбора
→ исходные записи / снимки
→ проекции Application
```

Слой Revit реализует физический сбор, но не определяет смысл `CollectionScope`.

- [`scope-collection.md`](scope-collection.md)
- [`chunked-collection.md`](chunked-collection.md)
- [`context-state.md`](context-state.md)

> Производные данные сбора должны оставаться связаны с тем состоянием Revit, из которого они получены.
