# Действия

Действия Application начинаются после получения найденного набора элементов.

Эта область не вызывает Revit API. Она рассчитывает нужный целевой набор и координирует вызов выходного порта.

- [`selection-and-visibility-calculation.md`](selection-and-visibility-calculation.md) — расчёт множеств;
- [`action-orchestration.md`](action-orchestration.md) — переход от результата фильтра к действию в Revit.

```text
найденный набор
→ расчёт Application
→ целевой набор / намерение действия
→ IRevitGateway
→ действие в Revit
```

`FilterResult` и результат выполнения действия в Revit — разные состояния.
