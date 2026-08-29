# UI

`ContextFilter.UI` — WPF-слой представления ContextFilter. Его ответственность — сделать Domain/Application возможности управляемыми для пользователя и хранить только тот state, который относится к текущей пользовательской сессии и представлению.

UI не определяет смысл фильтра, не реализует Application use cases и не обращается к Revit API напрямую.

## Граница слоя

```text
User
  ↓
View / ViewModel
  ↓
Application contracts / IRevitGateway
  ↓
async response
  ↓
UI state / feedback
```

Канонические владельцы:

- `domain/` — FilterDefinition, ParameterKey, scope и action semantics;
- `application/` — use cases, evaluation и orchestration;
- `infrastructure/` — settings/presets/history persistence;
- `revit/` — ExternalEvent, Revit API и host-side effects;
- `ui/` — представление, команды пользователя, UI state и feedback.

## Представление

Реализация поддерживает две формы:

- `DockablePane` — основной и default режим;
- `FloatingWindow` — альтернативное представление той же функциональности.

Основная рабочая область содержит:

1. выбор scope;
2. панель действий;
3. Category / Family / Type;
4. параметры;
5. значения и активные условия;
6. presets/history;
7. progress, matched count и refresh.

## Структура

```text
ui/
├─ interaction/   → как пользователь управляет фильтром и получает feedback
├─ state/         → ViewModel/session/document-bound UI state
└─ diagrams/      → UI flow
```

## Ключевые различия

```text
UI command
!= Application use case

UI selection state
!= Revit selection authority

visible filter condition
!= FilterDefinition ownership

matched count
!= host action success

loaded persisted settings
!= trusted runtime settings

pane lifetime
!= Revit document lifetime
```

## Навигация

1. [`interaction/`](interaction/) — экран, действия, presentation modes и guardrails.
2. [`state/`](state/) — ViewModels, lifecycle и document-bound state.
3. [`diagrams/ui-flow.puml`](diagrams/ui-flow.puml) — сквозной UI flow.
