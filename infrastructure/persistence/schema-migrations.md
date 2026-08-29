# Schema migrations

Persisted документы имеют собственную schema evolution, независимую от Revit document schema и Domain source-code layout.

## Подтверждённые миграции

### Settings schema v2

Переход к более спокойным defaults:

- auto-refresh — off;
- dynamic highlight — off;
- hotkeys — off.

Это изменение появилось как результат стабилизации поведения плагина в реальном Revit.

### Preset schema v2

Добавлены:

- `PresetKind`;
- различие `Full` / `Template`;
- встроенные шаблоны.

## Migration pipeline

```text
persisted document
→ inspect schema version
→ migrate to supported representation
→ validate / sanitize where applicable
→ expose through Application port
```

## Инварианты

- старая schema version не означает автоматически invalid user data;
- migration не должна менять Domain intent без явно определённого правила;
- current code model != persisted schema version;
- schema migration != business requirement migration.

## Ownership

`PresetDocumentMigrator` и migration mechanics принадлежат Infrastructure. Смысл `PresetKind` принадлежит Domain.
