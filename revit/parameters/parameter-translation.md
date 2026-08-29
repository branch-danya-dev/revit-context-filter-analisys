# Перевод параметров Revit ↔ Domain

Граница Revit переводит между представлением параметра в среде и стабильной идентичностью Domain.

```text
Revit Parameter / вычисляемый источник
→ RevitParameterKeyFactory
→ Domain ParameterKey

сырое значение Revit
→ RevitParameterValueConverter
→ значение Domain
```

Для создания штатного фильтра `ParameterKey` должен быть разрешён обратно в представление параметра Revit. Подтверждены `RevitParameterResolver` и `FilterDefinitionToElementFilterConverter`.

`RevitSyntheticParameterProvider` предоставляет `Category`, `Family`, `TypeName`, `ElementId`, `UniqueId`, `Workset`, `Level`.

```text
ParameterKey существует в Domain
!= штатный фильтр Revit способен его представить
```

Именно поэтому совместимость анализируется отдельно до преобразования в штатный фильтр.

Отображаемое имя — метаданные представления. Разрешение параметра должно опираться на идентичность, а не на локализованную или неуникальную подпись.
