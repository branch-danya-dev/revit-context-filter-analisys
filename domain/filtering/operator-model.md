# Filter Operator Model

Domain определяет 19 операторов, сгруппированных по semantic family.

| Семейство | Операторы |
|---|---|
| Equality | `Equals`, `NotEquals` |
| String | `Contains`, `NotContains`, `StartsWith`, `EndsWith` |
| Presence / Empty | `IsEmpty`, `IsNotEmpty`, `Exists`, `NotExists` |
| Numeric comparison | `GreaterThan`, `GreaterThanOrEqual`, `LessThan`, `LessThanOrEqual` |
| Range | `Between`, `NotBetween` |
| Membership | `InList`, `NotInList` |

## Operator != execution method

Оператор выражает semantic predicate.

Как именно он вычисляется — sequential scan, parallel scan, inverted lookup или другой эквивалентный алгоритм — не относится к Domain.

## Presence vs empty

```text
NotExists
!= IsEmpty

Exists
!= IsNotEmpty
```

Эти пары задают разные вопросы о property state.

## String operators

`FilterCondition` содержит `IgnoreCase`, поэтому case sensitivity является частью filter condition semantics, а не только UI-настройкой.

## Numeric and range operators

Source analysis подтверждает наличие numeric/range operators, но не фиксирует все детали:

- numeric tolerance;
- точные conversion rules;
- inclusivity границ `Between`;
- поведение для несовместимого `ParameterValueKind`.

Поэтому здесь они намеренно не конкретизируются без дополнительного evidence.

## Native compatibility

Корректный Domain operator не обязан иметь эквивалент в Revit `ElementFilter`.

Compatibility — отдельный вопрос Application/Revit boundary.
