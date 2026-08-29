# External Event

`external-event/` описывает единственный поддерживаемый мост между async UI/Application workflow и Revit API main thread.

## Pipeline

```text
UI async call
↓
IRevitGateway
↓
RevitExternalEventDispatcher
↓
RevitRequestQueue.Enqueue
↓
ExternalEvent.Raise
↓
ContextFilterExternalEventHandler.Execute
↓
RevitRequestDispatcher
↓
Revit service
↓
TaskCompletionSource result
```

## Почему это отдельная responsibility

Revit API не thread-safe. Поэтому любой workflow, который требует чтения или изменения Revit state, должен попасть в допустимый host execution context.

Application знает только порт. UI знает только async operation. Только Revit adapter знает, как безопасно превратить этот вызов в host request.

## Подтверждённые request types

- `Ping`;
- `RefreshContext`;
- `BuildParameterIndex`;
- `BuildParameterValues`;
- `ApplySelection`;
- `ApplyVisibility`;
- `ApplyNativeFilter`.

## Канонические документы

- [`main-thread-contract.md`](main-thread-contract.md)
- [`request-pipeline.md`](request-pipeline.md)
- [`request-coalescing.md`](request-coalescing.md)

## Инвариант

> Наличие async API в UI не даёт права обращаться к Autodesk Revit API из произвольного потока.
