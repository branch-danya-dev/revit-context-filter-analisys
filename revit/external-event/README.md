# ExternalEvent

`external-event/` описывает поддерживаемый мост между асинхронным UI/Application и главным потоком Revit API.

```text
асинхронный вызов UI
→ IRevitGateway
→ RevitExternalEventDispatcher
→ RevitRequestQueue.Enqueue
→ ExternalEvent.Raise
→ ContextFilterExternalEventHandler.Execute
→ RevitRequestDispatcher
→ сервис Revit
→ TaskCompletionSource result
```

Revit API не является потокобезопасным. Любая операция чтения или изменения Revit должна попасть в допустимый контекст среды.

Application знает порт, UI знает асинхронную операцию, а адаптер Revit знает, как превратить её в безопасный запрос среды.

Подтверждены типы запросов `Ping`, `RefreshContext`, `BuildParameterIndex`, `BuildParameterValues`, `ApplySelection`, `ApplyVisibility`, `ApplyNativeFilter`.

- [`main-thread-contract.md`](main-thread-contract.md)
- [`request-pipeline.md`](request-pipeline.md)
- [`request-coalescing.md`](request-coalescing.md)

> Наличие асинхронного API в UI не даёт права обращаться к Autodesk Revit API из произвольного потока.
