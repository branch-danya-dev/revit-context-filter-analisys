# Parameter translation

Revit boundary должен переводить между host-specific parameter representation и стабильной Domain identity.

## Поток чтения

```text
Revit Parameter / synthetic source
↓
RevitParameterKeyFactory
↓
Domain ParameterKey

Revit raw value
↓
RevitParameterValueConverter
↓
Domain parameter value
```

## Поток обратного разрешения

Для native-filter realization Domain `ParameterKey` должен быть разрешён обратно в Revit-compatible parameter representation.

Implementation analysis подтверждает `RevitParameterResolver` и `FilterDefinitionToElementFilterConverter` на action side.

## Synthetic properties

`RevitSyntheticParameterProvider` предоставляет такие filterable properties, как:

- Category;
- Family;
- TypeName;
- ElementId;
- UniqueId;
- Workset;
- Level.

Они могут участвовать в общем Domain filter language, хотя не каждый synthetic key соответствует native Revit `Parameter`.

## Ключевое различие

```text
Domain ParameterKey exists
!= native Revit filter can represent it
```

Именно поэтому native-filter compatibility анализируется отдельно до host conversion.

## Display names

Display label является presentation metadata. Разрешение параметра должно опираться на identity, а не на локализованную/неуникальную подпись.
