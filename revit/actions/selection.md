# Действие над выделением

Действие изменяет штатное выделение Revit на основании целевого набора, рассчитанного Application.

```text
FilterResult
→ SelectionSetCalculator
  Replace / Add / Exclude
→ целевые ElementId
→ IRevitGateway.ApplySelectionAsync
→ SelectionActionService
→ UIDocument.Selection.SetElementIds
```

Application определяет итоговый набор, Revit применяет его к текущему `UIDocument`.

```text
отмеченные узлы UI != выделение Revit
целевой набор Application != выделение Revit до успешного вызова API
```

Ошибка применения выделения не превращается в новый `FilterResult`: фильтрация могла быть корректной, а действие — нет.

Анализ также подтверждает переход/масштабирование к выделению внутри `SelectionActionService`; это удобство среды, а не семантика `Replace/Add/Exclude`.
