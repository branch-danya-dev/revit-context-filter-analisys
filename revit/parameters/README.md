# Параметры

`parameters/` описывает адаптацию параметров Autodesk Revit к стабильной модели Domain.

Подтверждены:

- `RevitElementSnapshotBuilder` — лёгкие снимки и отложенная загрузка параметров;
- `ParameterIndexService` — поддержка индекса и значений;
- `RevitParameterKeyFactory` — Revit `Parameter` ↔ Domain `ParameterKey`;
- `RevitParameterValueConverter` — значение Revit → значение Domain;
- `RevitSyntheticParameterProvider` — Category, Family, Level, Workset и другие вычисляемые свойства;
- `RevitTypeParameterCache` — повторное использование чтения параметров типа.

```text
Revit Element / Parameter
→ адаптер параметров Revit
→ ParameterKey + значение Domain
→ фильтрация Application
```

`ParameterKey` принадлежит Domain. Слой Revit отвечает за его корректное создание и обратное разрешение относительно конкретной модели.

- [`snapshot-building.md`](snapshot-building.md)
- [`parameter-translation.md`](parameter-translation.md)

> Удобная отображаемая подпись не может заменять стабильную идентичность параметра при переходе через границу Revit.
