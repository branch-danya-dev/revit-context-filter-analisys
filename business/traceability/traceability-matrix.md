# ContextFilter — traceability matrix

> Связывает подтверждённую потребность заказчика с нормализованными требованиями, областями системы и фактической валидацией.

## Принцип

Матрица не утверждает, что все строки существовали как формальные requirement IDs в исходных ТЗ.

IDs ниже созданы в публичной аналитической реконструкции для навигации и traceability.

| ID | Потребность / требование | Источник | Каноническая область | Проверка / evidence |
|---|---|---|---|---|
| FR-01 | Запуск ContextFilter из Revit | ТЗ + реализация | `business/processes`, `ui`, `revit` | рабочая команда Ribbon, финальное тестирование |
| FR-02 | Работа в ограниченном context | ТЗ + реализация | `business/scope`, `domain/context`, `application/context` | Active View / Entire Document / Current Selection |
| FR-03 | Category → Family → Type | оба ТЗ | `domain/context`, `application/context`, `ui` | пользовательская навигация по текущему context |
| FR-04 | Выбор параметра | расширенное ТЗ | `domain/parameters`, `application`, `ui` | доступные параметры текущего набора |
| FR-05 | Выбор / ввод значения | расширенное ТЗ | `domain/parameters`, `domain/filtering`, `ui` | quick-filter workflow |
| FR-06 | AND / OR | расширенное ТЗ | `domain/filtering` | filter engine tests + пользовательское поведение |
| FR-07 | Не выбирать посторонние типы | первоначальное ТЗ | `domain/filtering`, `application/filtering` | исправленная точность filter result |
| FR-08 | Dynamic highlight | расширенное ТЗ | `ui`, `application/actions`, `revit/actions` | opt-in live highlight workflow |
| FR-09 | Replace / Add / Exclude | расширенное ТЗ + реализация | `application/actions`, `revit/actions` | selection action behavior |
| FR-10 | Hide / Isolate / Inverse | оба ТЗ | `application/actions`, `revit/actions` | temporary visibility behavior |
| FR-11 | Native Revit filter | реализованный продукт | `application`, `revit/actions` | compatibility analysis + Revit realization |
| FR-12 | Full presets | расширенное ТЗ | `domain/presets`, `infrastructure/persistence` | save/load workflow |
| FR-13 | Template presets | расширенное ТЗ | `domain/presets`, `infrastructure/persistence` | structure reusable without fixed values |
| FR-14 | Видимое сообщение об ошибке | расширенное ТЗ + testing | `ui`, `system/review` | warning behavior |
| FR-15 | Не изменять BIM-элементы | расширенное ТЗ | `business/scope`, `revit/actions` | filtering/visibility do not edit source geometry/data |
| NFR-05 | Нормализовать invalid settings | user testing | `infrastructure/configuration` | stabilization fix |
| NFR-06 | Stop document-bound work | user testing | `revit/lifecycle`, `application/context` | stabilization fix |
| NFR-07 | Reset UI on document switch | user testing | `ui/state`, `revit/lifecycle` | stabilization fix |
| NFR-08 | Нет тяжёлого background work при закрытой панели | user testing | `application`, `revit/lifecycle` | stabilization/performance fix |
| NFR-09 | Не auto-open панель при старте Revit | user testing | `ui`, `revit/lifecycle` | stabilization fix |
| NFR-11 | Не конфликтовать с native Revit controls | user testing | `ui/interaction` | Ctrl+Click заменён, hotkeys opt-in |
| NFR-12 | Revit operations в корректном API context | user testing + implementation | `revit/external-event`, `revit/transactions` | isolation transaction fix |
| NFR-13 | Clean shutdown | user testing | `revit/lifecycle` | shutdown optimization |
| NFR-14 | Preset load failure visible | user testing | `infrastructure/persistence`, `ui` | warning added |

## Requirement evolution

```text
Initial customer request
→ fast Category / Family / Type actions

Extended requirement set
→ parameter filtering
→ selection context
→ AND / OR
→ presets / templates
→ richer actions

Implementation and user testing
→ lifecycle safety
→ settings validation
→ host interaction safety
→ performance stabilization
→ explicit failure feedback

Final state
→ director acceptance
→ deployment
→ regular use
```

## Где хранится истина

Эта матрица не дублирует детальную семантику компонентов.

Например:

- значение AND / OR канонически раскрывается в `domain/filtering/`;
- orchestration фильтрации — в `application/filtering/`;
- Revit transaction / ExternalEvent behavior — в `revit/`;
- persistence presets — в `infrastructure/`.

Матрица только связывает эти знания с исходной потребностью и проверкой.
