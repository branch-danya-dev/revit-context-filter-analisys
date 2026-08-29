# Actions

Application actions начинаются после получения matched element set.

Эта область не вызывает Revit API. Она вычисляет нужный target set и orchestrates вызов output port.

## Документы

- [`selection-and-visibility-calculation.md`](selection-and-visibility-calculation.md) — set calculations;
- [`action-orchestration.md`](action-orchestration.md) — переход от filter result к host action.

## Граница

```text
matched set
→ Application calculation
→ target intent
→ IRevitGateway
→ Revit side effect
```

`FilterResult` и результат Revit action — разные состояния.