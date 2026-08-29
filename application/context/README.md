# Context orchestration

Эта область описывает, как Application превращает полученный от Revit candidate set в производные структуры, необходимые UI и filtering engine.

## Документы

- [`context-orchestration.md`](context-orchestration.md) — путь получения и подготовки контекста;
- [`projection-pipeline.md`](projection-pipeline.md) — tree, parameter index и parameter values;
- [`cache-and-invalidation.md`](cache-and-invalidation.md) — многоуровневая зависимость derived state.

## Граница

Application не собирает `Element` через Revit API. Он запрашивает данные через порт и работает с Domain/Application representations.

```text
Revit collection
→ port response
→ Application projection/cache
→ UI + filtering
```