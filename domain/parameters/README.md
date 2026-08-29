# Domain Parameters

Параметр идентифицируется не только отображаемым именем.

```text
ParameterKey
= IdentityKind
+ IdentityValue
+ Source
```

`Source` различает Instance / Type / Synthetic.

Синтетические filterable properties включают Category, Family, TypeName, ElementId, UniqueId, Workset и Level.

Ключевые различия:

```text
display name != canonical identity
instance parameter != type parameter
synthetic property != native Revit parameter
missing != empty != not loaded
```
