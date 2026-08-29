# JSON stores

Infrastructure реализует Application storage ports через локальные JSON-файлы.

## `JsonSettingsStore`

Назначение:

```text
settings.json
→ deserialize
→ sanitize
→ runtime settings
```

Ключевой факт: настройки могут быть вручную изменены или содержать некорректные значения, поэтому чтение файла не завершает validation boundary.

## `JsonPresetStore`

Назначение:

```text
PresetDefinition[]
↔ persisted preset document
```

Store поддерживает schema migration и использует `AtomicFileWriter` для безопасной замены файла.

Semantic distinctions `Full` и `Template` принадлежат Domain; Infrastructure лишь сохраняет их representation.

## `JsonFilterHistoryStore`

Хранит recent filter history.

Подтверждённые mechanics:

- deduplication по signature;
- ограничение количества записей через `HistoryMaxItems`.

## Границы ошибок

Infrastructure должен различать:

```text
no persisted data
!= unreadable persisted data
!= invalid persisted data
!= valid empty collection
```

Точная политика восстановления каждого I/O failure в предоставленном source analysis полностью не описана, поэтому здесь она не выдумывается.
