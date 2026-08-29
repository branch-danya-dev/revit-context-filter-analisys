# Корень сборки зависимостей

`AddinHost` является точкой, где ContextFilter собирается в работающий граф внутри Revit.

Подтверждённый фрагмент регистрации:

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

DI-контейнер не является бизнес-архитектурой. Но место, где порты Application связываются с адаптерами Revit и запускаются внутри процесса Revit, принадлежит слою Revit.

```text
Application → объявляет порты и сервисы
Infrastructure → предоставляет адаптеры хранения
Revit AddinHost → собирает конкретный рабочий граф
```

`ContextFilterApplication.OnStartup()` инициирует `AddinHost.Initialize()`.

```text
регистрация сервиса
!= владение его ответственностью
```

То, что `FilterEvaluator` зарегистрирован здесь, не делает его ответственностью Revit; его канонический владелец — `application/filtering/`.
