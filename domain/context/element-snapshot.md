# Element Snapshot

`ElementSnapshot` — domain-представление одного Revit-элемента, используемое для клиентской фильтрации без постоянного обращения к Revit API.

Подтверждённая структура содержит:

```text
ElementSnapshot
├─ ElementId
├─ UniqueId
├─ CategoryIdentity
├─ FamilyTypeIdentity
├─ Parameters: ParameterKey → ParameterValue
└─ ParameterDisplayNames: ParameterKey → string
```

## Главная граница

```text
Revit Element
!=
ElementSnapshot
```

Revit `Element` остаётся host-owned объектом. Snapshot — производное представление данных, необходимых Domain/Application.

## Identity

Snapshot хранит как минимум:

- числовой `ElementId`;
- `UniqueId`;
- category identity;
- family/type identity.

Эти данные позволяют отделить identity элемента от отображаемых пользователю подписей.

## Parameters

Параметры представлены через:

```text
ParameterKey
→ ParameterValue
```

а display name хранится отдельно:

```text
ParameterKey
→ ParameterDisplayName
```

Это поддерживает ключевой инвариант:

> **Readable label не является canonical parameter identity.**

## Partial representation

Implementation analysis подтверждает использование light snapshots и последующую lazy-загрузку parameter data.

Из этого следует важное различие:

```text
parameter not present in current partial snapshot
!=
parameter proven to be missing on source element
```

Domain-модель не должна превращать техническую неполноту snapshot в бизнес-смысл `NotExists`.

## Инварианты

1. Snapshot не изменяет исходный Revit element.
2. Snapshot должен сохранять identity, достаточную для связи результата с source element.
3. Parameter lookup выполняется по `ParameterKey`, не по display name.
4. Partial snapshot должен отличаться по смыслу от доказанного отсутствия значения.
5. Любая стратегия построения snapshot обязана сохранять одинаковую domain semantics.
