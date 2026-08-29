# Selection action

Selection action изменяет native Revit selection на основании target set, рассчитанного Application layer.

## Поток

```text
FilterResult
↓
SelectionSetCalculator
  Replace / Add / Exclude
↓
target ElementIds
↓
IRevitGateway.ApplySelectionAsync
↓
SelectionActionService
↓
UIDocument.Selection.SetElementIds
```

Application определяет итоговый набор IDs с учётом action semantics. Revit layer применяет этот набор к текущему `UIDocument`.

## Authority

```text
UI checked nodes
!= Revit selection

Application target set
!= Revit selection until action succeeds
```

До успешного host call текущая selection authority остаётся у Revit.

## Failure boundary

Ошибку применения selection нельзя превращать в новый `FilterResult`. Фильтрация могла завершиться корректно, а host action — нет.

## Optional behavior

Implementation analysis также указывает zoom to selection в `SelectionActionService`. Это host-side convenience behavior и не входит в Domain semantics действия `Replace/Add/Exclude`.
