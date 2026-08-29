# Вспомогательные порты

Подтверждены `IPresetStore`, `IFilterHistoryStore`, `IPluginPreferences`, `ISettingsStore`, `IUiPresentationService`, `IDialogService`, `IAppLogger`.

```text
Порт Application
→ требуемая возможность

Адаптер Infrastructure / UI
→ конкретная реализация
```

`IPresetStore` не означает JSON, `IDialogService` не означает WPF-окно, `IAppLogger` не означает файловый журнал. Эти технологии принадлежат адаптерам.

Так Application сохраняет независимость от конкретного хранения и представления.
