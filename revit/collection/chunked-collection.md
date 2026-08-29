# Chunked collection

Большие Revit contexts нельзя безусловно читать одним долгим blocking operation, если это ухудшает responsiveness host application.

## Подтверждённая реализация

При количестве элементов выше `ChunkedCollectionThreshold` collection переключается на `ChunkedCollectionSession`.

Текущие implementation settings:

```text
threshold: 2500 elements
chunk size: 800 elements
Idling time budget: 45 ms
```

Эти числа являются runtime/configuration details, а не Domain semantics.

## Поток

```text
large candidate set
↓
start ChunkedCollectionSession
↓
Idling tick
↓
process bounded chunk
↓
report progress
↓
next Idling tick
↓
complete collection
```

## Ownership

- Revit layer владеет `Idling` и host-safe chunk execution;
- Application владеет тем, как полученные данные используются далее;
- UI только отображает progress;
- Domain не знает о chunk size или `Idling`.

## Session gate

Chunked work не должно продолжаться как постоянный background activity, когда пользовательская сессия ContextFilter не активна.

Implementation analysis подтверждает, что при `IsUserSessionActive == false` `Idling` не выполняет collect/index work.

## Инвариант

```text
chunking
changes execution timing

chunking
must not change collected semantic context
```
