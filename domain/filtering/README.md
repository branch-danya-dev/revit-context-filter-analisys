# Domain Filtering

Фильтр представлен логическим деревом.

```text
FilterDefinition
├─ Scope
├─ SelectedCategoryKeys
└─ RootGroup
    ├─ AND / OR
    ├─ Negate / IsEnabled
    └─ FilterCondition / nested FilterGroup
```

`FilterCondition` содержит `ParameterKey`, operator, operands и comparison policy.

Реализация поддерживает 19 операторов: equality, string, existence/emptiness, numeric, range и list operations.

Domain определяет **что означает фильтр**. Как его быстрее вычислить — ответственность Application.
