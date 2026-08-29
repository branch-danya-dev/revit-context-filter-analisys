# Context Tree

Для навигации candidate set представлен трёхуровневой иерархией:

```text
Category
└─ Family
   └─ Type
```

Типы узлов задаются `ContextNodeType`:

```text
Category
Family
Type
```

## Назначение

Дерево помогает пользователю сузить candidate set по предметно понятной структуре Revit-модели.

Каждый `ContextNode` содержит подтверждённые implementation analysis сведения:

- `ElementIds`;
- `TotalCount`;
- `IsChecked`;
- `DisplayName`;
- `Key`.

## Семантическая граница

```text
Category → Family → Type tree
!=
ownership hierarchy Revit
```

Это **derived navigation projection**, построенная над candidate elements.

Она не утверждает, что Category владеет Family или что Type является отдельным source authority.

## Инварианты

1. Узел дерева представляет подмножество candidate set.
2. Отметка узла влияет на выбор элементов для дальнейшего filter intent, но не меняет source model.
3. DisplayName не должен использоваться вместо стабильного key там, где требуется identity.
4. `TotalCount` — derived count, а не самостоятельное состояние элемента.
5. Порядок/визуальная группировка дерева не должны менять фактический element set.
