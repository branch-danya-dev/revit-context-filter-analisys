# Use case catalog

Подтверждённые реализацией Application use cases:

| Use case | Ответственность |
|---|---|
| `CollectContextUseCase` | выбрать доступный collection scope и инициировать получение контекста |
| `BuildContextTreeUseCase` | построить Category → Family → Type projection из `ElementTreeRecord[]` |
| `BuildParameterIndexUseCase` | построить каталог доступных параметров для snapshots |
| `BuildParameterValuesUseCase` | получить уникальные значения выбранного параметра |
| `BuildQuickFilterUseCase` | преобразовать выбор параметра/значений в `FilterDefinition` |
| `BuildPresetUseCase` | преобразовать текущее filter state в `PresetDefinition` |
| `SavePresetUseCase` | передать preset в persistence port |
| `DeletePresetUseCase` | удалить preset через store port |
| `ListPresetsUseCase` | прочитать доступные presets |
| `EnsureBuiltInPresetsUseCase` | обеспечить наличие встроенных templates |

## Группы сценариев

```text
Context preparation
→ CollectContext
→ BuildContextTree
→ BuildParameterIndex
→ BuildParameterValues

Filter intent
→ BuildQuickFilter

Reusable intent
→ BuildPreset
→ Save / List / Delete
→ EnsureBuiltInPresets
```

## Граница

Use cases управляют последовательностью действий. Они не становятся authority для Revit model state и не заменяют Domain definitions.