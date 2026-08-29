# Quick filter compilation

Quick filter — Application-механизм компиляции простого пользовательского выбора в canonical Domain filter tree.

```text
selected parameter
+ selected values
+ combine mode
        ↓
BuildQuickFilterUseCase
        ↓
FilterDefinition
```

Подтверждённые правила компиляции:

- `__missing__` → `NotExists`;
- `__empty__` → `IsEmpty`;
- одно обычное значение → `Equals`;
- несколько значений в OR-сценарии → `InList`;
- несколько значений при `combineValuesWithAnd=true` → AND-группа из `Equals`.

## Почему это Application

UI знает, что пользователь отметил значения. Domain знает, что означают `Equals`, `InList`, AND и `NotExists`. Application связывает эти два уровня и строит правильную Domain-структуру.

```text
UI representation
!= filter semantics
```

Quick filter не является альтернативным фильтрующим движком. После компиляции результат должен проходить через тот же evaluation pipeline, что и вручную построенное сложное дерево.