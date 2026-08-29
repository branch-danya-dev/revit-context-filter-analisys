# Evaluation pipeline

`FilterEvaluator` принимает Domain filter intent и in-memory element representations и возвращает `FilterResult`.

```text
FilterDefinition
+ ElementSnapshot[]
        ↓
normalize / prepare
        ↓
select evaluation strategy
        ↓
evaluate logical tree
        ↓
FilterResult
```

Application может компилировать logical tree в execution representation (`FilterEvaluationPlan`) с short-circuit behavior для AND/OR.

## Что здесь канонично

- evaluation работает над snapshots, а не напрямую над Revit elements;
- optimization может использовать compiled plan или index;
- результат должен соответствовать Domain operators и logical composition;
- execution failure нельзя интерпретировать как корректный zero-match result.

## Что здесь не канонично

Application не переопределяет truth semantics операторов. Если появляется новый operator или меняется его meaning, canonical change начинается в Domain, а затем переоткрывает evaluator implementation.