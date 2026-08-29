# Main-thread contract

Autodesk Revit API доступен ContextFilter только в разрешённом Revit execution context.

## Контракт

```text
caller thread
↓
Application port
↓
Revit request
↓
ExternalEvent
↓
Revit main thread
↓
Revit API
```

UI может быть asynchronous, но Revit API operation остаётся host-controlled synchronous execution.

## Следствия

1. ViewModel не вызывает Revit API напрямую.
2. Application use case не хранит `Document`, `View` или `UIDocument` как свой runtime authority.
3. Revit-specific read/write operation выполняется после dispatch в ExternalEvent handler.
4. Response возвращается обратно через async boundary.

## Что это защищает

Такой контракт предотвращает смешивание двух разных concurrency models:

```text
WPF / Task-based async model
!=
Revit API execution model
```

## Failure boundary

Если request невозможно выполнить в текущем host context, это не должно интерпретироваться как корректный пустой результат Domain/Application workflow.

```text
host execution failure
!= zero matches
```

Конкретная причина должна вернуться вызывающему слою как failure/response, после чего UI может показать её пользователю.
