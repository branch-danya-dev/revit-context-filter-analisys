# Native filter compatibility

`NativeFilterCompatibilityAnalyzer` отвечает на отдельный вопрос:

> можно ли текущий semantic `FilterDefinition` представить как штатный Revit `ElementFilter`?

Это не часть обычной client-side evaluation.

## Подтверждённый subset

Совместимыми в анализируемой реализации считаются:

- `Equals`;
- `NotEquals`;
- `GreaterThan`;
- `GreaterThanOrEqual`;
- `LessThan`;
- `LessThanOrEqual`;
- `InList`.

Ограничения включают:

- group `Negate`;
- `Contains`, `StartsWith`, `EndsWith`, `Between` и другие richer operators;
- большинство synthetic parameters;
- top-level OR, который не укладывается в поддерживаемую native representation.

## Главная граница

```text
semantic filter is valid
!= semantic filter is native-compatible
```

Compatibility analysis не должен обеднять основной filter language. Если native representation невозможна, client-side filter остаётся валидным; недоступным становится только конкретное native action.