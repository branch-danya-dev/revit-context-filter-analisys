# Хранение данных

Этот раздел описывает физическое хранение состояния ContextFilter и реализацию портов хранилищ Application.

Infrastructure владеет расположением файлов, JSON-сериализацией/десериализацией, атомарной записью, миграцией сохранённых схем, устранением дублей истории и ограничением её размера.

| Порт Application | Адаптер Infrastructure | Файл |
|---|---|---|
| `ISettingsStore` | `JsonSettingsStore` | `settings.json` |
| `IPresetStore` | `JsonPresetStore` | `presets.json` |
| `IFilterHistoryStore` | `JsonFilterHistoryStore` | `recent.json` |

Корень: `%AppData%\ContextFilter\`.

- [`storage-layout.md`](storage-layout.md)
- [`json-stores.md`](json-stores.md)
- [`atomic-write.md`](atomic-write.md)
- [`schema-migrations.md`](schema-migrations.md)

```text
успешная сериализация
!= успешно завершённая запись на диск
```
