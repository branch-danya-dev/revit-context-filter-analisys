# Persistence

Infrastructure хранит persisted state, включая presets, filter history и settings.

Persistence representation не становится каноническим смыслом Domain model.

```text
Domain model
↔ serialization / migration
↔ local persisted representation
```

После пользовательского тестирования сбой загрузки presets перестал быть silent failure: пользователю показывается предупреждение.
