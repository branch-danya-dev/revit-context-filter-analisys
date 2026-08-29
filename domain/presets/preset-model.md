# Preset Model

Подтверждённая domain-модель:

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

`PresetKind`:

```text
Full
Template
```

## Full preset

Full preset сохраняет filter intent вместе с конкретными значениями conditions.

Его задача — повторно применить уже сформулированный фильтр.

## Template preset

Template сохраняет структуру фильтра без обязательной привязки к конкретным значениям.

```text
parameter / condition structure
→ preserved

project-specific values
→ supplied later
```

Это позволяет использовать один semantic шаблон в другом текущем контексте.

## Preset != result

```text
PresetDefinition
→ load/extract
→ FilterDefinition
→ evaluate against current context
→ new FilterResult
```

Preset не должен хранить найденные ElementIds как канонический ответ на будущий запуск.

## Preset != persistence file

`presets.json` — инфраструктурное representation.

Domain владеет смыслом `PresetDefinition`; Infrastructure владеет безопасным сохранением, миграцией и чтением.

## Инварианты

1. Preset сохраняет intent, а не source Revit state.
2. Full и Template имеют различный semantic purpose.
3. Повторное применение preset происходит к текущему context, а не к историческому candidate set.
4. Schema/version metadata не должны подменять semantic content.
5. Ошибка persistence не должна интерпретироваться как корректный пустой список пользовательских intent без явного failure behavior.
