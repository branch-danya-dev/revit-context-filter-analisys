# Domain · Parameters

`parameters/` описывает **что считается параметром в filter semantics**, как сохраняется его identity и как различаются источник, значение и отображение.

## Документы

- [`parameter-identity.md`](parameter-identity.md) — `ParameterKey`, identity kind и source;
- [`value-model.md`](value-model.md) — `ParameterValue`, typed value и состояния missing/empty;
- [`synthetic-parameters.md`](synthetic-parameters.md) — Category, Family, TypeName и другие вычисляемые свойства.

## Основная модель

```text
ParameterKey
├─ IdentityKind
├─ IdentityValue
└─ Source

ParameterKey
↓
ParameterValue

ParameterKey
↓
DisplayName
```

Три связи намеренно разделены.

## Канонический принцип

```text
parameter label
!=
parameter identity
!=
parameter value
```
