# Application

Application слой отвечает за выполнение пользовательских сценариев поверх Domain-модели.

Он не определяет смысл фильтра и не работает с Revit API напрямую. Его ответственность — собрать шаги сценария, выбрать необходимые вычисления, вызвать порты и вернуть результат вызывающему слою.

```text
UI / external trigger
        ↓
Application use case
        ↓
Domain model + Application services
        ↓
Port
        ↓
Adapter (Revit / Infrastructure / UI)
```

## Каноническая ответственность

Application владеет:

- use cases;
- orchestration сценариев;
- построением производных представлений контекста;
- вычислением filter result;
- стратегиями evaluation;
- вычислением target sets для selection / visibility;
- анализом совместимости semantic filter с native Revit filter;
- портами к внешним реализациям;
- runtime cache и правилами его зависимостей.

Application не владеет:

- семантикой `FilterDefinition`, `ParameterKey`, `PresetDefinition` — это [`domain/`](../domain/);
- WPF state и presentation logic — это [`ui/`](../ui/);
- JSON persistence — это [`infrastructure/`](../infrastructure/);
- `ExternalEvent`, transactions и Revit API — это [`revit/`](../revit/).

## Структура

- [`use-cases/`](use-cases/) — входные сценарии Application;
- [`context/`](context/) — orchestration контекста, tree/index/value projections и cache;
- [`filtering/`](filtering/) — evaluation и compatibility analysis;
- [`actions/`](actions/) — вычисление и orchestration действий над matched set;
- [`ports/`](ports/) — границы Application с внешними адаптерами;
- [`diagrams/application-flow.puml`](diagrams/application-flow.puml) — общая схема Application pipeline.

## Главные различия

```text
Domain meaning
!= Application execution

use case
!= UI command

application port
!= adapter implementation

FilterResult
!= Revit side effect

cache hit
!= source authority

evaluation strategy
!= filter semantics
```

## Основной маршрут

```text
Collect context
→ Build tree / parameter index / values
→ Compile user choice into FilterDefinition
→ Evaluate
→ Calculate target action set
→ Call output port
```

Application должен оставаться тестируемым без живого Revit: внешние эффекты находятся за портами.