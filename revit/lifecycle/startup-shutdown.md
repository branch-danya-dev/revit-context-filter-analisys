# Startup and shutdown

## Startup

`ContextFilterApplication.OnStartup()` подтверждённо выполняет:

1. `AddinHost.Initialize()` — DI + ExternalEvent infrastructure;
2. регистрацию dockable pane;
3. создание Ribbon tab/button `ShowContextFilterCommand`;
4. подписку на Revit events: `ViewActivated`, `Idling`, `DocumentOpened`, `DocumentClosed`, `DocumentChanged`.

## Launch policy

После пользовательского тестирования plugin перестал автоматически открывать рабочую панель вместе с Revit. Пользователь запускает её через Ribbon.

Это отделяет:

```text
add-in loaded
!= user session active
```

## Shutdown

Implementation analysis фиксирует специальный быстрый teardown без `ServiceProvider.Dispose()`, потому что полное dispose приводило к задержкам/зависанию Revit.

Также stabilization history включает оптимизацию shutdown и освобождения ресурсов.

## Почему это системное решение

В обычном desktop app полное disposal может считаться стандартным выбором. В embedded add-in host behavior важнее универсальной привычки framework lifecycle.

```text
framework convention
!= safe Revit shutdown behavior
```

## Инвариант

> Teardown ContextFilter не должен удерживать или замедлять завершение Autodesk Revit.

Переданный source analysis не даёт полного списка всех освобождаемых ресурсов, поэтому документ не расширяет shutdown algorithm сверх подтверждённых решений.
