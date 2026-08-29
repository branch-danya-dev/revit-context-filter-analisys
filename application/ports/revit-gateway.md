# IRevitGateway

`IRevitGateway` — Application boundary для операций, которые требуют Autodesk Revit.

Подтверждённый контракт включает операции:

- получить текущую Revit context info;
- собрать context по `CollectionScope`;
- построить parameter index для element ids;
- получить values для `ParameterKey`;
- применить selection action;
- применить visibility action;
- применить native filter action.

```text
Application
→ IRevitGateway
→ Revit adapter
→ ExternalEvent / Revit API
```

## Почему gateway принадлежит Application

Application определяет, **какая capability ему нужна**. Revit layer определяет, **как capability безопасно реализовать в host**.

Поэтому:

```text
IRevitGateway
!= RevitExternalEventDispatcher
```

Первое — port contract. Второе — adapter implementation.

Это позволяет UI/Application tests использовать `FakeRevitGateway` без живого Revit.