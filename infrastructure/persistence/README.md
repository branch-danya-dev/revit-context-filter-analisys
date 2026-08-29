# Persistence

Этот раздел описывает физическое хранение состояния ContextFilter и реализацию Application store ports.

## Каноническая область

Infrastructure persistence владеет:

- расположением локальных файлов;
- JSON serialization/deserialization;
- atomic write mechanics;
- persisted-schema migration;
- history deduplication и retention limits.

Он не владеет смыслом `PresetDefinition` или настройками поведения системы.

## Реализованные stores

| Application port | Infrastructure adapter | Durable file |
|---|---|---|
| `ISettingsStore` | `JsonSettingsStore` | `settings.json` |
| `IPresetStore` | `JsonPresetStore` | `presets.json` |
| `IFilterHistoryStore` | `JsonFilterHistoryStore` | `recent.json` |

Физический root:

```text
%AppData%\ContextFilter\
```

Подробнее:

- [`storage-layout.md`](storage-layout.md)
- [`json-stores.md`](json-stores.md)
- [`atomic-write.md`](atomic-write.md)
- [`schema-migrations.md`](schema-migrations.md)

## Инвариант

```text
successful serialization
!= durable successful persistence
```

Для записей, где потеря/повреждение файла критично, Infrastructure должен завершить весь persistence protocol, а не только получить JSON string.
