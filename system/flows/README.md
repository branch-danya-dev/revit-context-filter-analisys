# Сквозные сценарии системы

Этот раздел показывает сценарии, которые пересекают несколько зон ответственности.

## Основные сценарии

- [`context-to-action.md`](context-to-action.md) — основной цикл сбор → фильтр → действие;
- [`document-lifecycle.md`](document-lifecycle.md) — изменение, закрытие и смена документа;
- [`preset-reuse.md`](preset-reuse.md) — сохранение и повторное применение условий фильтра;
- [`system-runtime.puml`](system-runtime.puml) — последовательность основного рабочего сценария.

Локальная логика каждого шага остаётся в соответствующем `domain/`, `application/`, `infrastructure/`, `ui/` или `revit/`.
