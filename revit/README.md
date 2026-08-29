# Revit

`ContextFilter.Revit` — слой-адаптер ContextFilter к Autodesk Revit 2025. Он связывает Domain/Application/UI со средой Revit и владеет ограничениями, возникающими именно из-за выполнения внутри Revit.

## Ответственность

Слой Revit отвечает за:

- вход через `IExternalApplication` и команду Ribbon;
- корень сборки зависимостей;
- безопасный доступ к Revit API через `ExternalEvent`;
- очередь и диспетчеризацию запросов;
- сбор элементов из `Document`, `View` и текущего выделения;
- преобразование элементов/параметров Revit в модели Domain;
- реальные действия над выделением, видимостью и штатными фильтрами;
- соблюдение требований транзакций;
- реакцию на события жизненного цикла Revit;
- работу `Idling`, ограниченную активной пользовательской сессией;
- завершение работы и интеграцию горячих клавиш.

## Граница владения состоянием

```text
Autodesk Revit
→ источник истины для Document / Element / View / Selection

Domain / Application
→ источник смысла фильтра и сценариев

ContextFilter.Revit
→ адаптер между этими областями
```

Слой Revit не определяет семантику `FilterDefinition`, `ParameterKey`, пресета или типа действия. Он реализует уже определённый смысл в рамках ограничений Revit API.

## Основная цепочка

```text
UI / Application
→ IRevitGateway
→ RevitExternalEventDispatcher
→ RevitRequestQueue
→ ExternalEvent.Raise()
→ ContextFilterExternalEventHandler
→ RevitRequestDispatcher
→ сервис Revit
→ Revit API
→ ответ
```

Revit API не является потокобезопасным, поэтому эта цепочка — системная граница, а не просто обёртка.

## Структура

```text
revit/
├─ external-event/   → запросы в главный поток Revit
├─ collection/       → сбор рабочего контекста из Revit
├─ parameters/       → перевод Revit ↔ Domain
├─ actions/          → реальные изменения среды
├─ transactions/     → граница записи
├─ lifecycle/        → запуск, события, сессия, завершение
└─ diagrams/         → схема интеграции с Revit
```

```text
Порт Application != адаптер Revit
Тип действия Domain != вызов Revit API
Корректный фильтр != штатный фильтр Revit
Асинхронный Task UI != разрешение обращаться к Revit API
Успешная фильтрация != успешное изменение Revit
Жизненный цикл документа != жизненный цикл панели
```
