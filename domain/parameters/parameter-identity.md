# Parameter Identity

Каноническая идентичность фильтруемого свойства задаётся `ParameterKey`:

```text
ParameterKey(
  IdentityKind,
  IdentityValue,
  Source
)
```

## IdentityKind

Implementation analysis подтверждает как минимум следующие семейства:

- `BuiltInParameter`;
- `SharedParameter`;
- `ProjectParameter`;
- `Synthetic`;
- другие поддерживаемые implementation-моделью виды identity.

## Source

`ParameterSource` различает:

```text
Instance
Type
Synthetic
```

Это принципиально: одно и то же читаемое имя не означает одинаковый semantic source.

## Почему display name недостаточно

В Revit одинаковая подпись параметра может встречаться у разных параметров или разных источников.

Поэтому:

```text
"Марка"
!= canonical identity
```

Фильтр должен ссылаться на `ParameterKey`, а UI может отдельно показывать локализованное/читаемое имя.

## Жизненный цикл identity

```text
Revit parameter / synthetic property
↓
resolve identity
↓
ParameterKey
↓
ElementSnapshot
↓
FilterCondition
↓
PresetDefinition
↓
optional Revit-native resolution
```

## Инварианты

1. `ParameterKey` переносит смысл параметра между Domain/Application boundary.
2. Display name не должен быть canonical lookup key.
3. Instance и Type parameter semantics не должны смешиваться без явного правила.
4. Synthetic parameter должен быть явно отличим от native Revit parameter.
5. Native Revit resolution может завершиться неуспешно, не делая сам `ParameterKey` семантически недействительным для client-side filter.
