# Application Filtering

`FilterEvaluator` вычисляет Domain `FilterDefinition` над `ElementSnapshot`.

Реализация использует три стратегии:

1. inverted index fast path;
2. sequential scan;
3. parallel scan.

Для сложных деревьев используется compiled evaluation plan с short-circuit AND / OR.

## Инвариант

```text
same fresh snapshots
+ same FilterDefinition
→ same matched set
```

Выбор стратегии — performance decision, а не изменение semantics.
