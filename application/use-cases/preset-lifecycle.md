# Preset lifecycle

Application управляет операциями вокруг `PresetDefinition`, не владея его Domain-смыслом и не владея физическим JSON-хранилищем.

```text
current filter state
→ BuildPresetUseCase
→ PresetDefinition
→ IPresetStore
→ Infrastructure adapter
```

Подтверждённые use cases:

- build;
- save;
- list;
- delete;
- ensure built-in presets.

В реализации присутствуют пять встроенных шаблонов для стен, дверей, окон, колонн и балок.

## Границы ownership

- `PresetDefinition`, `Full`, `Template` → Domain;
- orchestration preset lifecycle → Application;
- JSON, schema migration, atomic write → Infrastructure;
- отображение списка и user commands → UI.

```text
preset intent
!= persistence format
!= current FilterResult
```