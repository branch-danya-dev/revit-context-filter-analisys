# Модель действий

Domain содержит типы намерений, применимых к найденному набору элементов.

## SelectionAction
`Replace`, `Add`, `Exclude` означают заменить выделение, добавить к нему или исключить найденный набор. Расчёт множеств принадлежит Application, вызов `UIDocument.Selection` — Revit.

## VisibilityAction
`HideTemporary`, `IsolateTemporary`, `IsolateInverse`, `ResetTemporary` выражают намерение относительно временной видимости. Реализация принадлежит `revit/actions/`.

## NativeFilterAction
Подтверждены `Create`, `ReplaceExisting`; для конфликта присутствует `NativeFilterConflictResolution`: `Replace`, `Skip`, `Rename`.

```text
Тип действия Domain
!=
вызов Revit API
```

Domain описывает **что пользователь хочет сделать**. Слой Revit решает, как это допустимо выполнить в текущем API и транзакционном контексте.

Неуспешное действие в Revit не делает исходный `FilterResult` неправильным.
