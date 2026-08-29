# Domain Presets

`PresetDefinition` сохраняет reusable filter intent.

Поддерживаются два вида:

- **Full** — фильтр с конкретными значениями;
- **Template** — структура условий без фиксированных значений, которые выбираются в текущем проекте.

Preset может переживать смену рабочего Revit-контекста, потому что он хранит намерение пользователя, а не текущий matched set.

Физическое JSON-хранение принадлежит [`../../infrastructure/persistence/`](../../infrastructure/persistence/).
