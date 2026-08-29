# Binding lifecycle

WPF binding внутри Revit оказался не нейтральным presentation detail: при первоначальном DataContext bind property setters могли запускать side effects (`CollectAsync`, display-mode changes, live highlight), что приводило к fatal CLR `0xe0434352`.

## Исправленный lifecycle

Реализация использует `_suppressBindingSideEffects` и явные `BeginUiBind()` / `EndUiBind()`.

```text
create / attach view
↓
BeginUiBind
↓
bind DataContext and initial properties
↓
side effects suppressed
↓
EndUiBind
↓
normal user-driven side effects allowed
```

## Инвариант

> Initial UI materialization must not be interpreted as user intent.

То есть:

```text
binding sets property
!= user changed property
```

Это важное различие для embedded UI в host application: presentation initialization не должен самопроизвольно инициировать system workflows.

## Ownership

UI владеет suppression/deferred-binding behavior.

Revit-side ограничения безопасного attach и host lifecycle принадлежат `revit/`.

Application use cases при этом не должны знать, был ли вызов инициирован button click или WPF binding — UI обязан не отправлять ложный intent наружу.
