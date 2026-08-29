# Feedback and guardrails

UI является местом, где системное ограничение должно становиться понятным пользователю.

## Blocked scope

`ScopeOptionViewModel` поддерживает `BlockedReason`.

Это позволяет различать:

```text
option unavailable
!= option absent
```

Например, отсутствие подходящего active document/selection должно отображаться как конкретное ограничение текущего context, а не как молчаливое отсутствие результата.

## Large-context warning

`EntireDocument` может быть дорогим для больших моделей; реализация содержит warning threshold. Порог является runtime/configuration detail, но UI responsibility — показать предупреждение до дорогостоящего действия, а не менять смысл scope.

## Preset load failure

Ошибка восстановления preset не должна выглядеть как корректный пустой filter state.

```text
preset load fails
→ explicit warning
→ current valid UI state remains distinguishable
```

## Action failure

```text
FilterResult exists
→ action requested
→ downstream failure
→ visible failure feedback
```

Ошибка Revit action не должна интерпретироваться как `0 matches` или успешное выполнение.

## Progress

Длительные операции могут отображать progress. Progress является presentation of execution, а не владельцем chunking/ExternalEvent scheduling.

## Guardrail principle

> UI должен делать ограничения системы видимыми, а не скрывать их за пустым состоянием, silent fallback или изменением пользовательского intent.
