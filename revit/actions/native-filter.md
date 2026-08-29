# Native filter action

`NativeFilterActionService` пытается материализовать совместимый `FilterDefinition` как штатный Revit `ParameterFilterElement` и привязать его к view.

## Pipeline

```text
FilterDefinition
↓
Application NativeFilterCompatibilityAnalyzer
↓
compatible semantic subset
↓
IRevitGateway.ApplyNativeFilterAsync
↓
FilterDefinitionToElementFilterConverter
↓
RevitParameterResolver
↓
ElementFilter / ParameterFilterElement
↓
attach to View
```

## Подтверждённый compatibility subset

Application analysis допускает native conversion для:

- Equals;
- NotEquals;
- GreaterThan / GreaterThanOrEqual;
- LessThan / LessThanOrEqual;
- InList.

Не все Domain capabilities имеют native representation: negate, richer string operators, Between, большинство synthetic parameters и top-level OR могут быть несовместимы.

## Важная граница

```text
semantic filter
!= Revit ElementFilter
```

Native filter является optional realization, а не канонической формой фильтра ContextFilter.

## Parameter resolution

Даже совместимый оператор требует успешного разрешения Domain `ParameterKey` в Revit-compatible parameter identity. Поэтому operator compatibility и parameter resolvability — связанные, но разные проверки.

## Conflict handling

Domain содержит `NativeFilterAction / NativeFilterConflictResolution`, однако переданный source analysis не раскрывает полный conflict-resolution contract. Этот документ не придумывает точные сценарии replace/rename beyond подтверждённого факта создания/замены native filters.
