# Revit Parameter Mapping

Revit layer реализует перевод между native `Parameter` и Domain model:

- `RevitParameterKeyFactory`;
- `RevitParameterValueConverter`;
- `RevitSyntheticParameterProvider`;
- `RevitTypeParameterCache`;
- `RevitParameterResolver`.

```text
Revit representation
↔ Domain ParameterKey / ParameterValue
```

Физическая форма параметра в Revit не должна менять его Domain meaning.
