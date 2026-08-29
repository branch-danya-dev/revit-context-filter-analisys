# Регистрация зависимостей

Infrastructure предоставляет собственный набор регистрации для DI.

```text
AddContextFilterInfrastructure()
→ AppPaths
→ FileAppLogger
→ JsonSettingsStore
→ PluginPreferences
→ JsonPresetStore
→ JsonFilterHistoryStore
```

Сервисы, специфичные для Revit, регистрируются отдельно в `AddinHost.RegisterServices()`.

```text
регистрация Infrastructure
!= корень сборки всего приложения
```

Infrastructure знает, какие адаптеры реализуют его порты, но не становится владельцем композиции Revit.

Регистрация зависимостей — это связывание реализаций, а не владение системной ответственностью.
