# System Context

## Назначение

ContextFilter — add-in для Autodesk Revit 2025, который позволяет пользователю быстро построить контекстную выборку элементов, сформировать условия фильтрации и применить результат к Revit.

Система существует внутри уже открытого процесса Revit и не является самостоятельным редактором BIM-данных.

## Участники и внешняя среда

```text
BIM coordinator / designer
        ↓
ContextFilter
        ↓
Autodesk Revit 2025
        ↓
Revit document / views / elements / selection
```

## Что получает система от Revit

ContextFilter читает host context:

- текущий документ;
- активный вид;
- текущее пользовательское выделение;
- элементы и их Category / Family / Type;
- instance/type parameters;
- сведения, необходимые для synthetic properties вроде Category, Family, Level и Workset.

## Что система создаёт сама

ContextFilter создаёт собственные представления и intent:

- `ElementSnapshot`;
- `ParameterKey`;
- `FilterDefinition`;
- `FilterResult`;
- `PresetDefinition`;
- parameter indexes и derived cache;
- UI state.

Эти представления помогают анализировать host data, но не становятся authority для исходной Revit-модели.

## Что система возвращает в Revit

После вычисления результата плагин может:

- заменить / расширить / сократить selection;
- временно скрыть или изолировать элементы;
- выполнить inverse isolation;
- сбросить временную видимость;
- при совместимости создать штатный `ParameterFilterElement` и привязать его к виду.

## Принцип

> **ContextFilter интерпретирует и использует Revit-состояние, но не присваивает себе владение BIM-моделью.**
