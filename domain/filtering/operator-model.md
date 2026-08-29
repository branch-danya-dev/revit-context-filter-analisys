# Модель операторов фильтра

Domain определяет 19 операторов:

| Семейство | Операторы |
|---|---|
| Равенство | `Equals`, `NotEquals` |
| Строковые операции | `Contains`, `NotContains`, `StartsWith`, `EndsWith` |
| Наличие / пустота | `IsEmpty`, `IsNotEmpty`, `Exists`, `NotExists` |
| Числовое сравнение | `GreaterThan`, `GreaterThanOrEqual`, `LessThan`, `LessThanOrEqual` |
| Диапазон | `Between`, `NotBetween` |
| Принадлежность списку | `InList`, `NotInList` |

Оператор выражает логическое условие, а не способ его выполнения.

```text
NotExists != IsEmpty
Exists != IsNotEmpty
```

`IgnoreCase` делает чувствительность к регистру частью условия фильтра.

Источник подтверждает числовые и диапазонные операторы, но не фиксирует числовой допуск, точные правила преобразования, включённость границ `Between` и поведение для несовместимых `ParameterValueKind`. Эти детали намеренно не конкретизируются.

Корректный оператор Domain не обязан иметь эквивалент в Revit `ElementFilter`.
