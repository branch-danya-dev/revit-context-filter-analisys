# Filtering execution

Domain определяет, **что означает** `FilterDefinition`. Application отвечает за то, **как вычислить** его над набором `ElementSnapshot`.

## Документы

- [`evaluation-pipeline.md`](evaluation-pipeline.md) — общая evaluation contract;
- [`evaluation-strategies.md`](evaluation-strategies.md) — inverted / sequential / parallel;
- [`native-compatibility.md`](native-compatibility.md) — анализ возможности представить semantic filter как Revit native filter.

## Главный инвариант

```text
same FilterDefinition
+ same valid input snapshots
→ same semantic match set
```

Выбор optimization strategy не должен менять смысл фильтра.