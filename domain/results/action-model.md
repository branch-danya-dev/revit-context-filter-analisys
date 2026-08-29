# Action Model

Domain содержит типы намерений, которые могут быть применены к найденному element set.

## SelectionAction

```text
Replace
Add
Exclude
```

Semantic meaning:

- `Replace` — найденный набор становится новым selection;
- `Add` — найденный набор добавляется к существующему selection;
- `Exclude` — найденный набор исключается из существующего selection.

Конкретный расчёт set operations принадлежит Application, а вызов `UIDocument.Selection` — Revit layer.

## VisibilityAction

```text
HideTemporary
IsolateTemporary
IsolateInverse
ResetTemporary
```

Эти значения выражают user intent относительно временной видимости.

Конкретная реализация Revit temporary hide/isolate принадлежит `revit/actions/`.

## NativeFilterAction

Подтверждённые action values:

```text
Create
ReplaceExisting
```

Для конфликта имени/существующего native filter используется `NativeFilterConflictResolution`:

```text
Replace
Skip
Rename
```

## Главная граница

```text
Domain action enum
!=
Revit API call
```

Domain описывает **что пользователь хочет сделать**. Revit layer решает, как это допустимо выполнить внутри host transaction/API constraints.

## Инварианты

1. `SelectionAction` не должен изменять source model geometry/parameters.
2. Temporary visibility action относится к view state, а не к filter semantics.
3. Native filter creation возможна только если filter definition совместим с Revit representation.
4. Неуспешная host action не превращает исходный filter result в неправильный.
