# Snapshot building

`RevitElementSnapshotBuilder` преобразует live Revit elements в `ElementSnapshot`, который далее может использоваться клиентским filter engine без постоянного обращения к Revit API.

## Двухэтапная модель

Implementation analysis подтверждает:

```text
Revit Element
↓
light snapshot
  category
  family
  type
↓
lazy parameter load when needed
↓
parameter-complete snapshot/index data
```

## Зачем light snapshot

Пользователь сначала работает с контекстом и Category → Family → Type. Читать полный набор параметров каждого элемента заранее может быть дорого на больших моделях.

Поэтому deeper parameter data загружается по необходимости.

## Семантическое следствие

```text
parameter not loaded yet
!= parameter missing in Revit
```

Revit layer должен отличать отсутствие загруженной информации от подтверждённого отсутствия source parameter.

## Authority

`ElementSnapshot` — derived representation.

```text
ElementSnapshot
!= Revit Element authority
```

Если Revit document изменился, snapshot может стать stale независимо от того, что его объект всё ещё существует в памяти.

## Type parameters

Реализация использует `RevitTypeParameterCache` для повторного использования type-level reads. Это performance mechanism и не меняет различие `Instance` / `Type` в Domain `ParameterSource`.
