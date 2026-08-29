# Context orchestration

`CollectContextUseCase` выбирает scope с учётом доступности и запускает получение контекста через Application boundary.

```text
requested CollectionScope
        ↓
availability check
        ↓
IRevitGateway.CollectContextAsync(...)
        ↓
RevitContextInfo / collected data
        ↓
derived projections
```

Подтверждённый пример: отсутствие открытого документа делает соответствующий сценарий недоступным; Application не должен пытаться компенсировать отсутствие host context.

После получения candidate set Application может инициировать:

1. построение Category → Family → Type tree;
2. построение parameter index;
3. загрузку values конкретного `ParameterKey`;
4. дальнейшую filter evaluation.

## Важная граница

```text
requested scope
!= collected context
!= derived tree/index
```

Domain определяет `CollectionScope`; Revit реализует физический сбор; Application связывает это в пользовательский workflow.