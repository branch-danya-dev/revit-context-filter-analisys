# Request pipeline

Revit request pipeline реализует `IRevitGateway` через host-specific dispatcher.

## Поток

```text
IRevitGateway method
↓
RevitExternalEventDispatcher
↓
RevitRequest
↓
RevitRequestQueue
↓
ExternalEvent.Raise()
↓
ContextFilterExternalEventHandler.Execute()
↓
RevitRequestDispatcher.Execute(request)
↓
specialized Revit service
↓
response
```

## Request vocabulary

Подтверждённые операции охватывают весь host boundary:

| Request | Host responsibility |
|---|---|
| `Ping` | проверить доступность Revit context |
| `RefreshContext` | прочитать текущий candidate context |
| `BuildParameterIndex` | получить host parameter data |
| `BuildParameterValues` | получить значения параметра |
| `ApplySelection` | изменить Revit selection |
| `ApplyVisibility` | изменить temporary visibility |
| `ApplyNativeFilter` | создать/применить native filter |

## Что request не делает

Request type не является Domain command language. Например:

```text
FilterDefinition
!= ApplyNativeFilter request
```

Первый объект описывает смысл фильтра, второй — конкретную попытку реализовать часть этого смысла через Revit capability.

## Response boundary

Host-side service возвращает response обратно в async caller. Для action flow implementation analysis подтверждает ответ с count/message, который затем превращается UI в `ActionFeedbackMessage`.
