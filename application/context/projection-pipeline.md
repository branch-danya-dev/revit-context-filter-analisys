# Context projection pipeline

Application строит несколько производных представлений одного candidate context.

```text
ElementTreeRecord[]
→ BuildContextTreeUseCase
→ Category / Family / Type tree

ElementSnapshot[]
→ BuildParameterIndexUseCase
→ parameter catalog

ParameterKey
→ BuildParameterValuesUseCase
→ unique values
```

Эти представления отвечают на разные вопросы и не должны смешиваться:

- tree — навигация и ограничение candidate subset;
- parameter index — какие параметры доступны в текущем наборе;
- parameter values — какие значения доступны для выбранного параметра;
- snapshots — данные, по которым evaluation проверяет условия.

## Инвариант

Derived projection не становится source authority. Если исходный Revit context изменился и цепочка инвалидирована, старое tree/index/value state не должно считаться актуальной истиной.