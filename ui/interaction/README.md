# UI Interaction

Этот раздел описывает пользовательское взаимодействие с ContextFilter: как пользователь открывает инструмент, выбирает контекст, формирует filter intent, запускает действия и получает feedback.

## Канонический flow

```text
Open ContextFilter
→ choose scope
→ inspect Category / Family / Type
→ choose parameter
→ choose values / conditions
→ evaluate
→ inspect matched count
→ apply selection / visibility / native filter action
```

UI инициирует этот flow, но не владеет семантикой его шагов.

## Документы

- [`screen-model.md`](screen-model.md) — структура основной рабочей области;
- [`user-actions.md`](user-actions.md) — пользовательские действия и их границы;
- [`presentation-modes.md`](presentation-modes.md) — DockablePane и FloatingWindow;
- [`feedback-and-guardrails.md`](feedback-and-guardrails.md) — предупреждения, blocked state и ошибки;
- [`host-interaction.md`](host-interaction.md) — взаимодействие с native Revit semantics и hotkeys.

## Инвариант

> Plugin interaction должен дополнять host application, а не незаметно перехватывать его устоявшуюся семантику.
