# Configuration

Этот раздел описывает жизненный цикл persisted settings и границу между сохранённой конфигурацией и безопасным runtime state.

## Основная цепочка

```text
settings.json
→ deserialize
→ migrate
→ sanitize / normalize
→ runtime configuration
```

## Ответственность

Infrastructure отвечает за:

- чтение/запись settings document;
- schema migration;
- sanitization некорректных значений;
- предоставление конфигурации через Application ports.

Infrastructure не определяет, что означает конкретная Domain-модель или Revit operation.

Подробнее:

- [`settings-lifecycle.md`](settings-lifecycle.md)
- [`sanitization.md`](sanitization.md)
- [`calm-defaults.md`](calm-defaults.md)

## Главный invariant

```text
persisted value
!= trusted runtime value
```

Пользовательский файл — это input boundary. Он может быть старой версии, вручную изменён, частично некорректен или содержать чрезмерные значения.
