# Supporting ports

Кроме Revit gateway, Application использует порты для других внешних capabilities.

Подтверждены:

- `IPresetStore`;
- `IFilterHistoryStore`;
- `IPluginPreferences`;
- `ISettingsStore`;
- `IUiPresentationService`;
- `IDialogService`;
- `IAppLogger`.

## Ownership

```text
Application port
→ required capability

Infrastructure / UI adapter
→ concrete realization
```

Примеры:

- `IPresetStore` не означает JSON — JSON является Infrastructure implementation;
- `IDialogService` не означает WPF window — конкретное presentation решение находится за границей;
- `IAppLogger` не означает file logger — файловая запись является Infrastructure detail.

Так Application сохраняет независимость от persistence и presentation technology.