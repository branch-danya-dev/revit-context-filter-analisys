# Application Actions

Application рассчитывает намерение над matched set до host-specific записи.

В реализации существуют:

- `SelectionSetCalculator` — Replace / Add / Exclude;
- `VisibilitySetCalculator` — в том числе inverse isolation.

```text
FilterResult
→ action calculation
→ Revit port
→ native host operation
```

Application не владеет `UIDocument.Selection`, `View.HideElements` или Revit transactions — это [`../../revit/`](../../revit/).
