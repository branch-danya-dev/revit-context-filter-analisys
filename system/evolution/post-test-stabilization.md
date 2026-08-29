# Post-test Stabilization

После первой рабочей реализации пользовательское тестирование выявило ряд проблем, которые потребовали не только локальных bugfixes, но и уточнения системных правил.

| Наблюдение | Переоткрытое предположение | Системное изменение |
|---|---|---|
| Некорректные пользовательские настройки приводили к ошибкам | persisted config можно использовать напрямую | validation + normalization перед runtime use |
| Сбой загрузки presets не был виден пользователю | persistence failure можно оставить silent | explicit warning / failure visibility |
| Background operations продолжались после закрытия документа | document-bound work может переживать host context | stop obsolete work on document lifecycle |
| UI state сохранялся после смены документа | UI session независима от document identity | reset document-specific UI state |
| Hotkeys были активны всегда | глобальный shortcut безопасен по умолчанию | hotkeys становятся opt-in |
| Плагин автоматически открывался при запуске Revit | постоянная активность приемлема | запуск только по явной команде пользователя |
| `Ctrl + Click` конфликтовал с Revit multi-select | plugin gesture не конфликтует с host interaction | `Ctrl + Shift + Click`, конфликтный gesture отключён по умолчанию |
| Event handlers создавали нагрузку без активного использования | handlers могут работать постоянно | session-bound event activity |
| Built-in presets создавались при старте Revit | initialization должен происходить при application startup | lazy initialization при первом открытии панели |
| Isolation выполнялась вне необходимого transaction context | action semantics достаточно для исполнения | host transaction constraints становятся частью execution contract |
| Shutdown Revit задерживался | background resources освободятся автоматически | explicit shutdown/resource cleanup |

## Архитектурный результат

Из тестирования сформировались несколько более общих правил:

```text
host lifecycle
→ ограничивает valid runtime state

persisted data
→ требует validation

plugin convenience
→ не должна ломать native Revit interaction

inactive feature
→ не должна создавать постоянную тяжёлую нагрузку

failure
→ должен быть видим и отличим от valid empty result
```

Эти правила закреплены в [`../invariants/system-invariants.md`](../invariants/system-invariants.md).
