# Domain · Параметры

`parameters/` описывает, **что считается параметром в семантике фильтра**, как сохраняется его идентичность и как различаются источник, значение и отображение.

- [`parameter-identity.md`](parameter-identity.md) — `ParameterKey`, вид идентичности и источник;
- [`value-model.md`](value-model.md) — `ParameterValue`, типизированное значение и состояния отсутствия/пустоты;
- [`synthetic-parameters.md`](synthetic-parameters.md) — Category, Family, TypeName и другие вычисляемые свойства.

```text
ParameterKey
├─ IdentityKind
├─ IdentityValue
└─ Source

ParameterKey → ParameterValue
ParameterKey → DisplayName
```

```text
подпись параметра
!= идентичность параметра
!= значение параметра
```
