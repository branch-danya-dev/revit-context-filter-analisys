# Evaluation strategies

Реализация содержит три режима выполнения `FilterEvaluator`.

## 1. Inverted index fast path

Применяется к поддерживаемым простым формам фильтра; source analysis отдельно называет `Equals`, `InList`, `NotExists`, `IsEmpty`.

`InvertedParameterIndex` позволяет переиспользовать lookup по normalized parameter value для того же snapshot set.

## 2. Sequential scan

Используется для сложных деревьев или небольших candidate sets. Logical tree компилируется в `FilterEvaluationPlan` и выполняется с short-circuit AND/OR.

## 3. Parallel scan

Для достаточно больших candidate sets реализация может выполнять evaluation параллельно. В анализируемой версии указан threshold 1500 элементов.

## Системное правило

```text
strategy choice
→ performance decision

strategy choice
≠ semantic decision
```

Fast path допустим только если он доказуемо эквивалентен общей evaluation semantics.

Thresholds являются configuration/runtime detail и могут меняться без изменения Domain model.