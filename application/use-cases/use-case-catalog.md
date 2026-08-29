# Каталог сценариев использования

| Сценарий | Ответственность |
|---|---|
| `CollectContextUseCase` | выбрать доступную область и инициировать получение контекста |
| `BuildContextTreeUseCase` | построить Категория → Семейство → Тип из `ElementTreeRecord[]` |
| `BuildParameterIndexUseCase` | построить каталог параметров для снимков |
| `BuildParameterValuesUseCase` | получить уникальные значения выбранного параметра |
| `BuildQuickFilterUseCase` | преобразовать выбор параметра/значений в `FilterDefinition` |
| `BuildPresetUseCase` | преобразовать текущее состояние фильтра в `PresetDefinition` |
| `SavePresetUseCase` | передать пресет в порт хранения |
| `DeletePresetUseCase` | удалить пресет через порт хранилища |
| `ListPresetsUseCase` | прочитать доступные пресеты |
| `EnsureBuiltInPresetsUseCase` | обеспечить наличие встроенных шаблонов |

```text
Подготовка контекста
→ CollectContext
→ BuildContextTree
→ BuildParameterIndex
→ BuildParameterValues

Условия фильтра
→ BuildQuickFilter

Повторное использование
→ BuildPreset
→ Save / List / Delete
→ EnsureBuiltInPresets
```

Сценарии управляют последовательностью действий, но не становятся источником истины для состояния Revit и не заменяют определения Domain.
