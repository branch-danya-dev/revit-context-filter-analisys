# Filter Definition

`FilterDefinition` — корневая domain-структура пользовательского filter intent.

Подтверждённая модель:

```text
FilterDefinition
├─ Scope: CollectionScope
├─ RootGroup: FilterGroup
└─ SelectedCategoryKeys
```

## Семантика

Фильтр отвечает на три вопроса:

```text
ГДЕ искать?
→ Scope

КАКОЙ category/type context учитывать?
→ SelectedCategoryKeys

КАКИЕ условия должны быть истинны?
→ RootGroup
```

## FilterDefinition не является результатом

```text
FilterDefinition
+ Candidate Set
→ evaluation
→ FilterResult
```

Один и тот же definition может дать разные результаты для разных candidate sets или разных состояний Revit-модели.

## FilterDefinition не является UI state

Quick filter, checked values и WPF controls могут **порождать** `FilterDefinition`, но каноническим смыслом после компиляции является domain definition.

## FilterDefinition не является native Revit filter

Native `ParameterFilterElement` — только одна возможная техническая реализация части filter intent.

Некоторые корректные domain filters могут не иметь эквивалентного native representation.

## Инварианты

1. Filter intent должен быть выражаем без зависимости от Revit API objects.
2. Scope является частью intent.
3. Логическая структура условий должна сохраняться, а не заменяться плоским списком.
4. Selected category context должен быть отделён от parameter conditions.
5. Изменение evaluation strategy не должно менять смысл definition.
