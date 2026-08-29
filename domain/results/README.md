# Domain · Результаты и действия

`results/` отделяет **результат вычисления фильтра** от **намерения выполнить действие над найденным набором**.

- [`filter-result.md`](filter-result.md) — смысловая роль `FilterResult`;
- [`action-model.md`](action-model.md) — типы действий над выделением, видимостью и штатным фильтром.

```text
FilterResult
!= SelectionAction
!= VisibilityAction
!= NativeFilterAction
```

Один найденный набор может использоваться разными действиями без изменения смысла фильтра.
