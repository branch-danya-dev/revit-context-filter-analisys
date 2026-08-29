# Модель пресета

Подтверждённая модель:

```text
PresetDefinition
├─ SchemaVersion
├─ Id
├─ Name
├─ Description
├─ CollectionScope
├─ PresetKind
├─ RootGroup
└─ CategoryKeys
```

`PresetKind`: `Full`, `Template`.

## Full
Сохраняет условия фильтра вместе с конкретными значениями для повторного применения.

## Template
Сохраняет структуру фильтра без обязательной привязки к конкретным значениям, которые выбираются в новом контексте.

```text
PresetDefinition
→ восстановить FilterDefinition
→ вычислить в текущем контексте
→ новый FilterResult
```

Пресет не должен хранить найденные `ElementId` как канонический ответ на будущий запуск.

`presets.json` — инфраструктурное представление. Domain владеет смыслом `PresetDefinition`, Infrastructure — безопасным сохранением и миграцией.
