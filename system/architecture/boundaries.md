# System Boundaries

## Внутри границы ContextFilter

В систему входят:

- Domain-модель фильтра, параметров, presets и результатов;
- Application use cases и evaluation services;
- persistence настроек, presets и history;
- WPF UI и его interaction state;
- Revit add-in entry point;
- ExternalEvent / request queue;
- collectors, converters и action services;
- lifecycle coordination;
- derived cache, indexes и performance coordination.

## За пределами границы

Внешним authority остаётся Autodesk Revit:

```text
Revit owns
→ Document
→ Elements
→ Views
→ UIDocument.Selection
→ native ParameterFilterElement state
→ transaction rules
→ host event lifecycle
```

Плагин не владеет исходной геометрией или параметрами элементов и не должен произвольно изменять их как часть filter workflow.

## Граница UI ↔ Revit API

WPF не может безопасно обращаться к Revit API как к обычному async backend.

Поэтому системная граница выглядит так:

```text
WPF interaction
→ Application request
→ IRevitGateway / request queue
→ ExternalEvent
→ valid Revit API context
→ response
→ UI feedback
```

`ExternalEvent` здесь не является бизнес-операцией. Это механизм безопасного пересечения host boundary.

## Клиентская фильтрация и native filter

Два представления фильтра нужно разделять:

```text
FilterDefinition
→ канонический пользовательский intent

ParameterFilterElement
→ возможное native-представление части этого intent
```

Не каждый корректный ContextFilter filter обязан быть представим штатным Revit filter.

## Persistence boundary

Настройки, presets и history хранятся вне Revit document в `%AppData%\ContextFilter`.

Они принадлежат плагину, но не являются подтверждением текущего состояния документа. Persisted intent может быть повторно применён к новому Revit context только после нового разрешения параметров и вычисления результата.
