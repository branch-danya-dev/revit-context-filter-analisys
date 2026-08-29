# Цепочка запроса Revit

`IRevitGateway` реализуется через диспетчер `ExternalEvent`.

```text
метод IRevitGateway
→ RevitExternalEventDispatcher
→ RevitRequest
→ RevitRequestQueue
→ ExternalEvent.Raise()
→ ContextFilterExternalEventHandler.Execute()
→ RevitRequestDispatcher.Execute(request)
→ специализированный сервис Revit
→ ответ
```

| Запрос | Ответственность Revit |
|---|---|
| `Ping` | проверить доступность контекста |
| `RefreshContext` | прочитать текущих кандидатов |
| `BuildParameterIndex` | получить данные параметров |
| `BuildParameterValues` | получить значения параметра |
| `ApplySelection` | изменить выделение Revit |
| `ApplyVisibility` | изменить временную видимость |
| `ApplyNativeFilter` | создать/применить штатный фильтр |

Тип запроса не является языком команд Domain.

```text
FilterDefinition
!= ApplyNativeFilter request
```

Сервис Revit возвращает ответ асинхронному вызывающему коду. Для действий подтверждён ответ с количеством/сообщением, который затем отображается UI.
