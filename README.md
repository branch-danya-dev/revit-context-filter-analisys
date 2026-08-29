# ContextFilter — системный анализ

[RU](README.md) · [EN](README_EN.md)

> Реальный кейс системного анализа, проектирования, разработки и внедрения плагина контекстной фильтрации для Autodesk Revit 2025.

## Контекст проекта

| | |
|---|---|
| **Моя роль** | System Analyst · Solution Designer · Developer |
| **Заказчик** | Заместитель директора проектного института |
| **Предметный эксперт** | BIM-координатор |
| **Запрос** | Расширенный аналог Bimstep / ModPlus для быстрой контекстной фильтрации элементов Revit |
| **Приёмка** | Финальный результат принимал директор института |
| **Результат** | Плагин внедрён и используется; после внедрения ошибок и претензий не поступало |

Название организации намеренно не публикуется.

## Задача

Пользователю нужен быстрый способ работать не со всей BIM-моделью сразу, а с выбранным контекстом: активным видом, всем документом или текущим выделением. Внутри этого контекста система должна позволять найти элементы по категории, семейству, типу и параметрам, а затем применить результат к Revit.

```text
Revit document
      ↓
Collection scope
      ↓
Context elements
      ↓
Filter definition
      ↓
Matched element set
      ↓
Selection / visibility / native Revit filter
```

## Архитектура решения

Реализованное решение построено как Clean Architecture из пяти проектов:

```text
                 ContextFilter.UI
                       │
                       ▼
              ContextFilter.Application
                       │
                       ▼
                 ContextFilter.Domain

ContextFilter.Infrastructure ──→ Application + Domain

ContextFilter.Revit ───────────→ все слои
        │
        └─ Revit API / ExternalEvent / collectors / actions / lifecycle
```

- **Domain** — модели и семантика фильтрации;
- **Application** — use cases, orchestration, filter evaluation и порты;
- **Infrastructure** — persistence, settings, logging и DI infrastructure;
- **UI** — WPF / DockablePane / ViewModels и пользовательское взаимодействие;
- **Revit** — add-in entry point, ExternalEvent, Revit API, collectors, native actions и host lifecycle.

## Структура репозитория

Репозиторий построен по принципам [SSAD](https://github.com/branch-danya-dev/ssad-methodology) и по той же логике, что [Aveli System Analysis](https://github.com/branch-danya-dev/aveli-system-analysis): сначала бизнес и система в целом, затем реальные технические зоны ответственности.

```text
business/        → зачем существует продукт, требования, scope, процессы, traceability
system/          → архитектура, flows, данные, инварианты, evolution и validation

domain/          → каноническая модель фильтра, параметров, snapshots и presets
application/     → use cases, evaluation, orchestration, ports и set calculations
infrastructure/  → persistence, configuration и logging
ui/              → WPF interaction model и UI state
revit/           → Revit API boundary, collection, ExternalEvent, actions и lifecycle
```

## Главные системные различия

```text
Revit element
!= in-memory ElementSnapshot

parameter display name
!= ParameterKey identity

parameter missing
!= parameter empty
!= parameter not loaded

FilterDefinition
!= evaluation strategy

matched element set
!= action applied to Revit

valid semantic filter
!= necessarily convertible native Revit filter
```

## Реальный цикл разработки

```text
Customer requirements
        ↓
Clarification with customer + BIM coordinator
        ↓
System analysis
        ↓
Solution design
        ↓
Implementation
        ↓
User testing
        ↓
Observed runtime / UX / host-integration issues
        ↓
System corrections
        ↓
Director acceptance
        ↓
Deployment
```

После пользовательского тестирования были, в частности, уточнены lifecycle документа, валидация настроек, работа фоновых обработчиков, горячие клавиши, загрузка пресетов, транзакционность Revit-действий и освобождение ресурсов при завершении.

## Навигация

Начать с:

1. [`business/`](business/) — зачем и для кого создавался ContextFilter;
2. [`system/`](system/) — как решение устроено целиком;
3. [`domain/`](domain/) — что означает фильтр и его данные;
4. [`application/`](application/) — как выполняются пользовательские сценарии;
5. [`revit/`](revit/) — как решение безопасно работает внутри Autodesk Revit.
