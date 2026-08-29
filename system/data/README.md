# System Data View

Ключевые модели, проходящие через систему:

```text
CollectionScope
CollectedContext
ElementSnapshot
ParameterKey / ParameterValue
FilterDefinition
FilterResult
PresetDefinition
```

## Источник и derived state

```text
Revit document data
→ authoritative source facts

ElementSnapshot / context tree / parameter index / filter result
→ derived application state
```

Derived state должен быть связан с актуальным документом и рабочим контекстом и не может считаться новым источником истины.
