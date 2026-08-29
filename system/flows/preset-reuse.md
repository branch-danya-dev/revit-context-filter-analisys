# Preset Reuse Flow

Preset позволяет сохранить filter intent и повторно использовать его позже.

## Сохранение

```text
current UI/filter state
→ BuildPresetUseCase
→ PresetDefinition
→ preset store
→ presets.json
```

Поддерживаются два вида:

- `Full` — включает конкретные значения;
- `Template` — сохраняет структуру условий без жёсткой привязки к значениям.

## Повторное применение

```text
persisted PresetDefinition
→ load
→ restore filter intent
→ resolve against current context
→ evaluate again
→ new FilterResult
```

Preset не хранит «вечный» результат фильтрации.

## Почему это важно

Один и тот же intent может быть применён в другом документе, виде или selection context.

Следовательно:

```text
saved intent
!= current document state
!= current matched element set
```

Persisted preset является authority для сохранённого пользовательского intent, но не для существования параметров или элементов в текущем Revit document.

## Failure visibility

После пользовательского тестирования ошибка загрузки presets перестала быть silent failure: пользователю должно быть показано предупреждение о невозможности восстановить сохранённый intent.
