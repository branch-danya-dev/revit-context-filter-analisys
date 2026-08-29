# Выполнение фильтрации

Domain определяет, **что означает** `FilterDefinition`. Application отвечает за то, **как вычислить** его над набором `ElementSnapshot`.

- [`evaluation-pipeline.md`](evaluation-pipeline.md) — общий контракт вычисления;
- [`evaluation-strategies.md`](evaluation-strategies.md) — инвертированный индекс, последовательный и параллельный проход;
- [`native-compatibility.md`](native-compatibility.md) — возможность представить фильтр как штатный фильтр Revit.

```text
один FilterDefinition
+ одни и те же актуальные снимки
→ один и тот же семантический набор совпадений
```

Выбор оптимизации не должен менять смысл фильтра.
