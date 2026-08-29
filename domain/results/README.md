# Domain · Results and Actions

`results/` отделяет **результат вычисления фильтра** от **намерения выполнить действие над найденным набором**.

## Документы

- [`filter-result.md`](filter-result.md) — semantic role `FilterResult`;
- [`action-model.md`](action-model.md) — Selection / Visibility / Native Filter action enums.

## Центральное различие

```text
FilterResult
!=
SelectionAction
!=
VisibilityAction
!=
NativeFilterAction
```

Один matched set может использоваться разными действиями без изменения смысла самого фильтра.
