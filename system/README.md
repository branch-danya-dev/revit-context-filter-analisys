# System

Этот раздел показывает **ContextFilter как одну систему**, а не как набор solution-проектов.

## Основной путь

```text
User intent
→ UI
→ Application use case
→ Domain semantics
→ Revit realization
→ observable result
```

Infrastructure поддерживает persistence, configuration и logging, но не определяет смысл фильтрации.

## Содержание

- [`architecture/`](architecture/) — компоненты и зависимости;
- [`flows/`](flows/) — сквозные сценарии;
- [`data/`](data/) — ключевые модели и derived state;
- [`invariants/`](invariants/) — правила, которые должны сохраняться независимо от реализации;
- [`evolution/`](evolution/) — изменения после пользовательского тестирования;
- [`review/`](review/) — verification и acceptance.

Локальная техническая истина находится у соответствующих владельцев: [`../domain/`](../domain/), [`../application/`](../application/), [`../infrastructure/`](../infrastructure/), [`../ui/`](../ui/) и [`../revit/`](../revit/).
