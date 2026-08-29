# Request coalescing

`RevitRequestQueue` не обязан выполнять каждый промежуточный request, если более новый request того же latest-only класса уже сделал старый неактуальным.

## Подтверждённые coalescing cases

Implementation analysis указывает схлопывание повторных:

- `RefreshContext`;
- `BuildParameterIndex`;
- live-highlight selection requests.

Принцип:

```text
pending request A
↓
newer equivalent request B arrives
↓
A is no longer the desired current state
↓
keep latest relevant request
```

## Зачем это нужно

ExternalEvent pipeline является ограниченным host resource. Без coalescing быстрые UI changes могут создать очередь действий, которые к моменту выполнения уже устарели.

## Семантическая граница

Coalescing допустим только для операций, где промежуточное состояние не является самостоятельным обязательным событием.

```text
latest-only refresh
can be coalesced

user-required durable mutation
must not be silently discarded
```

Переданный source analysis не перечисляет политику coalescing для всех request types, поэтому этот документ не расширяет её за подтверждённые cases.

## Связь с freshness

Coalescing оптимизирует доставку актуального intent, но сам по себе не доказывает freshness данных:

```text
latest queued request
!= current Revit snapshot
```

Freshness по-прежнему зависит от document/view/selection state и lifecycle invalidation.
