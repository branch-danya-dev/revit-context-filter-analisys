# Dependency registration

Infrastructure предоставляет собственный registration bundle для DI.

Подтверждённая registration model:

```text
AddContextFilterInfrastructure()
→ AppPaths
→ FileAppLogger
→ JsonSettingsStore
→ PluginPreferences
→ JsonPresetStore
→ JsonFilterHistoryStore
```

Revit-specific services регистрируются отдельно в `AddinHost.RegisterServices()`.

## Почему это важно

```text
Infrastructure registration
!= application composition root
```

Infrastructure знает, какие adapters реализуют его порты, но не должна становиться владельцем Revit host composition.

## Dependency direction

```text
Infrastructure
→ Application abstractions
→ Domain
```

Revit composition root затем связывает Infrastructure, Application, UI и Revit adapters в работающий add-in.

## Инварианты

- Infrastructure не зависит от конкретной WPF ViewModel как от своего контракта;
- JSON adapter можно заменить без изменения Domain semantics;
- Revit-specific registrations не должны протекать в Infrastructure package;
- DI registration является wiring, а не системным ownership.
