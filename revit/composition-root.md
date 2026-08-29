# Composition Root

`AddinHost` является composition root ContextFilter внутри Revit.

## Что здесь собирается

Реализация регистрирует Infrastructure adapters, Application services, Revit-specific services, gateway и UI ViewModel в одном runtime container.

Подтверждённый фрагмент composition root включает:

```text
AddContextFilterInfrastructure()
RevitRequestQueue
ContextCollectionCache
IFilterEvaluator → FilterEvaluator
ContextCollectionService
ParameterIndexService
IRevitGateway → RevitExternalEventDispatcher
MainPaneViewModel
```

## Почему owner — Revit

Сам DI container не является бизнес-архитектурой. Но точка, в которой конкретные Application ports связываются с Revit adapters и запускаются внутри Revit process, принадлежит host layer.

```text
Application
→ declares ports and services

Infrastructure
→ provides file/settings adapters

Revit AddinHost
→ composes concrete runtime graph
```

## Startup relation

`ContextFilterApplication.OnStartup()` инициирует `AddinHost.Initialize()`, после чего доступны ExternalEvent infrastructure, dockable pane и ribbon command.

Composition root не должен менять meaning объектов Domain. Его задача — построить работоспособный runtime graph.

## Важное различие

```text
service registration
!= service ownership
```

То, что `FilterEvaluator` зарегистрирован в Revit composition root, не делает его Revit responsibility. Каноническая ответственность evaluator остаётся в `application/filtering/`.

Аналогично `JsonPresetStore` может быть создан через общий container, но его реализация принадлежит `infrastructure/`.
