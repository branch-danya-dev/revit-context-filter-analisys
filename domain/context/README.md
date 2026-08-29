# Domain · Контекст

`context/` описывает **семантическую область элементов, над которой может быть построен фильтр**, и представления Domain после получения данных из Revit.

## Документы

- [`collection-scope.md`](collection-scope.md) — три допустимые области;
- [`context-model.md`](context-model.md) — `CollectedContext`, идентичность и происхождение контекста;
- [`element-snapshot.md`](element-snapshot.md) — представление элемента в памяти;
- [`context-tree.md`](context-tree.md) — Категория → Семейство → Тип как навигационная проекция.

```text
CollectionScope
↓
множество идентификаторов кандидатов
↓
CollectedContext
↓
ElementSnapshot
↓
данные, пригодные для фильтрации
```

Как именно Revit собирает элементы, выполняет локальное обновление и порционный сбор, а кэш физически инвалидируется — это `application/` + `revit/`.
