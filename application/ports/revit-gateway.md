# IRevitGateway

`IRevitGateway` — граница Application для операций, требующих Autodesk Revit.

Подтверждённый контракт включает получение текущего контекста Revit, сбор по `CollectionScope`, построение индекса/значений параметров и применение выделения, видимости и штатного фильтра.

```text
Application
→ IRevitGateway
→ адаптер Revit
→ ExternalEvent / Revit API
```

Application определяет, **какая возможность ему нужна**. Слой Revit определяет, **как безопасно реализовать её в среде**.

```text
IRevitGateway
!= RevitExternalEventDispatcher
```

Первое — контракт порта, второе — конкретный адаптер. Благодаря этому тесты могут использовать `FakeRevitGateway` без живого Revit.
