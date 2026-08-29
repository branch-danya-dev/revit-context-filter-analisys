# UI State

UI state — это представление текущей пользовательской сессии, а не копия Revit document и не каноническая Domain-модель.

## Что хранит UI

UI может хранить:

- выбранный scope option;
- раскрытие/выбор узлов context tree;
- текущий parameter selection;
- выбранные values;
- active filter conditions для отображения;
- matched count;
- busy/progress/error state;
- выбранный preset/history item;
- presentation/settings controls.

## Что UI не делает authority

```text
UI tree state
!= candidate-set authority

UI parameter label
!= ParameterKey identity

UI matched count
!= FilterResult ownership

UI selected action
!= committed Revit effect
```

## Lifecycle

Document-bound state должен быть валиден только для document context, из которого он получен.

```text
Document A
→ UI projection A

switch to Document B
→ projection A invalid
→ reset document-bound UI state
→ rebuild from B when requested/allowed
```

## Документы

- [`viewmodel-responsibilities.md`](viewmodel-responsibilities.md) — распределение state по ViewModels;
- [`session-state.md`](session-state.md) — активная пользовательская сессия и optional live behavior;
- [`binding-lifecycle.md`](binding-lifecycle.md) — безопасный WPF binding;
- [`document-transition.md`](document-transition.md) — reset при смене/закрытии документа.
