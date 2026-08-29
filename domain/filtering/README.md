# Domain · Filtering

`filtering/` хранит канонический **filter language ContextFilter**.

Здесь определяется, что пользователь хочет найти, но не то, каким алгоритмом Application вычислит результат.

## Документы

- [`filter-definition.md`](filter-definition.md) — корневая модель фильтра;
- [`logical-composition.md`](logical-composition.md) — `FilterGroup`, `FilterCondition`, AND/OR, negate, enabled state;
- [`operator-model.md`](operator-model.md) — 19 операторов и их semantic families.

## Главная граница

```text
Filter meaning
!=
Evaluation strategy
!=
Native Revit representation
```

`FilterEvaluator`, inverted index, sequential/parallel scan и compatibility analyzer относятся к Application/Revit.
