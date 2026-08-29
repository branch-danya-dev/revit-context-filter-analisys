# Действие штатного фильтра Revit

`NativeFilterActionService` пытается материализовать совместимый `FilterDefinition` как штатный `ParameterFilterElement` и привязать его к виду.

```text
FilterDefinition
→ NativeFilterCompatibilityAnalyzer
→ совместимая часть семантики
→ IRevitGateway.ApplyNativeFilterAsync
→ FilterDefinitionToElementFilterConverter
→ RevitParameterResolver
→ ElementFilter / ParameterFilterElement
→ привязка к View
```

Подтверждённая совместимость включает `Equals`, `NotEquals`, `GreaterThan`, `GreaterThanOrEqual`, `LessThan`, `LessThanOrEqual`, `InList`.

`Negate`, более богатые строковые операторы, `Between`, большинство вычисляемых параметров и верхнеуровневый OR могут не иметь штатного представления.

```text
фильтр ContextFilter
!= Revit ElementFilter
```

Штатный фильтр — дополнительная реализация, а не каноническая форма `FilterDefinition`.

Даже совместимый оператор требует успешного разрешения `ParameterKey` в параметр Revit. Совместимость оператора и разрешимость параметра — разные проверки.

Domain содержит `NativeFilterAction` / `NativeFilterConflictResolution`, но переданный анализ не раскрывает полный контракт разрешения конфликтов. Точные сценарии переименования/замены сверх подтверждённых фактов не придумываются.
