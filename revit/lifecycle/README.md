# Lifecycle

`lifecycle/` описывает связь ContextFilter с жизненным циклом Autodesk Revit и текущего document/session context.

## Host lifecycle responsibilities

Revit layer отвечает за:

- `OnStartup` / `OnShutdown` add-in;
- регистрацию ribbon и dockable pane;
- подписку на `ViewActivated`, `Idling`, `DocumentOpened`, `DocumentClosed`, `DocumentChanged`;
- session-bound background work;
- document/view/selection transitions;
- hotkey hooks, активные только в допустимом host context;
- освобождение ресурсов без зависания Revit.

## Документы

- [`startup-shutdown.md`](startup-shutdown.md)
- [`events-and-session.md`](events-and-session.md)
- [`hotkeys.md`](hotkeys.md)

## Основной принцип

```text
plugin process lifetime
!= current document lifetime
!= active user session lifetime
```

Эти три времени жизни пересекаются, но не являются одним и тем же.

## Validation evidence

Пользовательское тестирование выявило, что background operations могли продолжаться после закрытия документа, UI state мог переживать document switch, а постоянные event handlers создавали лишнюю нагрузку. После стабилизации работа была привязана к активной пользовательской сессии и host lifecycle.
