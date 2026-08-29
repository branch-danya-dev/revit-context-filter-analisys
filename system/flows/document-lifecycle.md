# Document Lifecycle Flow

ContextFilter работает с derived state, построенным из конкретного Revit document. Поэтому изменение host context должно переоткрывать зависимые знания.

## DocumentChanged

Реализация использует debounced обработку изменений модели.

```text
DocumentChanged
→ debounce
→ оценка масштаба изменения
→ incremental patch, если изменение локальное
   OR
→ invalidate / mark stale, если безопасный patch невозможен
```

Patch и full invalidate — разные стратегии восстановления актуальности, но цель у них одна: не выдавать старый derived context за текущий.

## Active View change

Смена активного вида меняет universe для `ActiveView` scope.

Следовательно, должны быть переоткрыты зависящие от него:

```text
candidate set
→ tree
→ parameter index / values
→ filter result
```

## Selection change

Для `CurrentSelection` selection является частью context identity. Изменение selection делает старый selection-scoped context неактуальным.

## Document close / switch

После закрытия или смены документа:

- document-bound background work должен быть остановлен;
- старый context не должен продолжать использоваться;
- document-specific UI state должен быть сброшен;
- Revit event work не должен продолжать тяжёлую обработку для уже неактуального документа.

Это правило появилось как результат реального пользовательского тестирования.

## Session activity

Когда пользовательская сессия ContextFilter неактивна, Idling не должен выполнять тяжёлые collect/index/highlight операции.

```text
panel closed / session inactive
→ no heavy background work
```

Это системное ограничение производительности и lifecycle, а не просто локальная оптимизация.
