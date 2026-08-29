# Visibility actions

`VisibilityActionService` реализует temporary visibility operations в текущем Revit view.

Подтверждённые Domain intents:

- `HideTemporary`;
- `IsolateTemporary`;
- `IsolateInverse`;
- `ResetTemporary`.

## Поток

```text
matched set
↓
Application VisibilitySetCalculator
↓
target set / action intent
↓
IRevitGateway.ApplyVisibilityAsync
↓
VisibilityActionService
↓
Revit View temporary visibility API
```

`IsolateInverse` особенно хорошо показывает разделение ответственности: Application вычисляет «всё кроме matched», а Revit adapter реализует полученный target set через host API.

## Transaction requirement

В пользовательском тестировании был обнаружен реальный дефект: isolation выполнялась вне требуемого Revit transaction context и завершалась ошибкой. Реализация была исправлена.

Отсюда системный invariant:

> Корректный semantic action не отменяет требований host API к execution/transaction context.

## Failure boundary

```text
valid target set
!= successful visibility mutation
```

Если Revit не разрешает action в текущем состоянии, UI должен получить явную ошибку, а не выглядеть так, будто фильтр вернул пустой набор.
