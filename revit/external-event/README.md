# ExternalEvent Boundary

Revit API не является thread-safe и должен вызываться из допустимого Revit execution context.

Реализованный путь:

```text
UI async request
→ IRevitGateway
→ RevitRequestQueue
→ ExternalEvent.Raise()
→ ExternalEventHandler.Execute()
→ RevitRequestDispatcher
→ host-specific service
→ TaskCompletionSource result
```

Повторяющиеся pending requests могут coalesce'иться, чтобы в host pipeline оставалась актуальная работа.
