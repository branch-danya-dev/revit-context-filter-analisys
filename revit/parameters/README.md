# Parameters

`parameters/` описывает адаптацию Autodesk Revit parameters к стабильной Domain-модели ContextFilter.

## Подтверждённые компоненты

- `RevitElementSnapshotBuilder` — light snapshots + lazy parameter load;
- `ParameterIndexService` — host-side support для index/values/inverted indexes;
- `RevitParameterKeyFactory` — Revit Parameter ↔ Domain `ParameterKey`;
- `RevitParameterValueConverter` — Revit value → Domain value;
- `RevitSyntheticParameterProvider` — Category, Family, Level, Workset и другие synthetic properties;
- `RevitTypeParameterCache` — reuse type-parameter reads.

## Граница

```text
Revit Element / Parameter
↓
Revit parameter adapter
↓
ParameterKey + Domain value
↓
Application filtering
```

`ParameterKey` принадлежит Domain. Revit layer отвечает за то, чтобы корректно создать/разрешить этот key относительно конкретного Revit source.

## Документы

- [`snapshot-building.md`](snapshot-building.md)
- [`parameter-translation.md`](parameter-translation.md)

## Инвариант

> Удобное display name не может заменять стабильную identity параметра при переходе через Revit boundary.
