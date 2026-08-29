# Filter Result

`IFilterEvaluator` в Domain contract возвращает `FilterResult`:

```text
Evaluate(elements, definition, cancellation)
→ FilterResult
```

## Семантическая роль

`FilterResult` представляет результат применения `FilterDefinition` к конкретному candidate set.

```text
Candidate Set
+
FilterDefinition
↓
FilterResult
```

Он является **derived state** и зависит как минимум от:

- текущего candidate set;
- filter definition;
- доступных parameter values.

## Что источник не подтверждает

Предоставленный `PROJECT_ANALYSIS.md` не содержит определения полей `FilterResult`.

Поэтому этот документ намеренно **не придумывает**:

- точные свойства объекта;
- порядок элементов;
- наличие статистики/diagnostics;
- структуру ошибок внутри результата.

Эти детали должны быть добавлены только после проверки исходного Domain type или дополнительных evidence.

## Инварианты

1. Filter result не является source authority для Revit elements.
2. Zero matches является валидным результатом только при успешной evaluation.
3. Evaluation failure не должен молча выглядеть как valid empty result.
4. Результат относится к конкретному context/filter state и может устареть после изменения source model.
5. Действие над результатом не меняет семантику самого `FilterResult`.
