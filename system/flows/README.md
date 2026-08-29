# System Flows

Этот раздел показывает сценарии, которые пересекают несколько responsibility owners.

## Основные flows

- [`context-to-action.md`](context-to-action.md) — основной цикл collect → filter → action;
- [`document-lifecycle.md`](document-lifecycle.md) — изменение / закрытие / смена документа;
- [`preset-reuse.md`](preset-reuse.md) — сохранение и повторное применение filter intent;
- [`system-runtime.puml`](system-runtime.puml) — sequence основного runtime flow.

Локальная логика каждого шага остаётся в соответствующем `domain/`, `application/`, `infrastructure/`, `ui/` или `revit/`.
