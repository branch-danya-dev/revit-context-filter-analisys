# Domain · Фильтрация

`filtering/` хранит канонический **язык фильтра ContextFilter**.

Здесь определяется, что пользователь хочет найти, но не то, каким алгоритмом Application вычислит результат.

## Документы

- [`filter-definition.md`](filter-definition.md) — корневая модель фильтра;
- [`logical-composition.md`](logical-composition.md) — `FilterGroup`, `FilterCondition`, AND/OR, `Negate`, `IsEnabled`;
- [`operator-model.md`](operator-model.md) — 19 операторов и их смысловые семейства.

```text
Смысл фильтра
!=
Способ вычисления
!=
Штатное представление Revit
```

`FilterEvaluator`, инвертированный индекс, последовательный/параллельный проход и анализ совместимости относятся к Application/Revit.
