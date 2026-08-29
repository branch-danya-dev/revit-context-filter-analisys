# Transaction boundary

Transaction boundary определяется не пользовательской кнопкой, а требованиями конкретного Revit API operation.

## Не открывать transaction слишком рано

Основной filter pipeline включает много read/compute steps:

```text
collect source data
→ build snapshots/index
→ evaluate FilterDefinition
→ calculate target set
```

Эти шаги не должны автоматически превращаться в одну длинную Revit write transaction.

## Открывать transaction там, где начинается host mutation

```text
prepared action intent
↓
Revit action service
↓
if operation requires transaction
    start valid Revit transaction
    perform mutation
    commit / handle failure
```

## Почему граница важна

Смешивание вычисления и host mutation приводит к двум проблемам:

1. transaction живёт дольше фактической write responsibility;
2. ошибку Revit mutation сложнее отделить от корректного filter result.

## Validation finding

В реальном тестировании isolation была реализована вне необходимой transaction mechanics и завершалась ошибкой. После исправления action стал соблюдать Revit host contract.

## Что source не подтверждает

Переданный анализ не содержит точной transaction matrix для каждого метода `SelectionActionService`, `VisibilityActionService` и `NativeFilterActionService`. Поэтому здесь не утверждается, что все actions требуют одинаковой transaction strategy.

Каноническое правило:

> Каждый Revit adapter operation должен применять transaction ровно в соответствии с контрактом используемого Revit API.
