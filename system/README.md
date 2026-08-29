# System

Этот раздел показывает **ContextFilter как одну систему**, а не как набор проектов solution.

Он отвечает на вопросы:

- где проходит граница плагина относительно Autodesk Revit;
- какие крупные части системы существуют и за что отвечают;
- как данные и пользовательский intent проходят через Domain, Application, Infrastructure, UI и Revit;
- какие сквозные инварианты должны сохраняться;
- какие части модели пришлось пересмотреть после пользовательского тестирования;
- как проверяется согласованность требований, модели и реализации.

## Карта раздела

- [`architecture/`](architecture/) — контекст, границы и component model;
- [`flows/`](flows/) — сквозные сценарии между слоями;
- [`data/`](data/) — ownership и движение ключевых данных;
- [`invariants/`](invariants/) — правила, которые должны оставаться истинными во всей системе;
- [`evolution/`](evolution/) — изменения модели после реального пользовательского тестирования;
- [`review/`](review/) — verification и consistency review.

## Система в одном представлении

```text
USER
  ↓
UI
  ↓
APPLICATION
  ↓
DOMAIN

INFRASTRUCTURE
  ↗

REVIT
→ host entry point
→ source data
→ native actions
```

Фактическое направление зависимостей реализации:

```text
Revit → UI, Infrastructure, Application, Domain
UI → Application, Domain
Infrastructure → Application, Domain
Application → Domain
Domain → ∅
```

Это направление зависимостей не означает, что Domain владеет Revit-состоянием. Autodesk Revit остаётся authority для документа, элементов, видов, selection и native API state.

## Главный системный поток

```text
Revit context
→ collect
→ normalize / project
→ build filter intent
→ evaluate in memory
→ matched element set
→ execute selected action in Revit
```

Подробные локальные модели находятся у своих владельцев: [`../domain/`](../domain/), [`../application/`](../application/), [`../infrastructure/`](../infrastructure/), [`../ui/`](../ui/), [`../revit/`](../revit/).
