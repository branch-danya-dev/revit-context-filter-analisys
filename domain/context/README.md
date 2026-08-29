# Domain · Context

`context/` описывает **семантическую область элементов, над которой может быть построен фильтр**, и domain-представления, используемые после получения данных из Revit.

## Документы

- [`collection-scope.md`](collection-scope.md) — три допустимых scope;
- [`context-model.md`](context-model.md) — `CollectedContext`, identity и provenance контекста;
- [`element-snapshot.md`](element-snapshot.md) — in-memory representation элемента;
- [`context-tree.md`](context-tree.md) — Category → Family → Type как навигационная domain-проекция.

## Каноническая цепочка

```text
CollectionScope
↓
Candidate element identity set
↓
CollectedContext
├─ ElementIds
├─ provenance / cache identity
└─ Category → Family → Type projection

Element identity
↓
ElementSnapshot
↓
filterable domain data
```

## Не здесь

Как именно Revit собирает элементы, как выполняется incremental patch, когда запускается chunked collection и как кэш инвалидируется физически — это `application/` + `revit/`.
