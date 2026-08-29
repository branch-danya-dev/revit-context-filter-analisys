# Domain · Presets

Preset — это **сохраняемый reusable filter intent**, а не сохранённый результат фильтрации и не JSON-файл.

Основная модель описана в [`preset-model.md`](preset-model.md).

```text
PresetDefinition
→ reusable intent

JsonPresetStore
→ persistence representation
```

Persistence lifecycle и schema migration реализуются в `infrastructure/`, но semantic distinction `Full | Template` принадлежит Domain.
