# Calm defaults

Settings schema v2 закрепила более консервативные defaults для поведения, которое может создавать постоянную нагрузку или вмешиваться в host interaction.

Подтверждённые defaults:

```text
auto-refresh      = off
dynamic highlight = off
hotkeys           = off
```

## Причина

Эти defaults отражают опыт реального тестирования ContextFilter в Revit:

- автоматические фоновые операции могли создавать лишнюю нагрузку;
- постоянный highlight плохо масштабируется на больших наборах;
- глобальные/low-level hotkeys могут конфликтовать с Revit и другими add-ins.

## Смысл

```text
feature exists
!= feature must run by default
```

Infrastructure хранит и мигрирует эти defaults. Runtime gating и host-specific поведение принадлежат Application/Revit, а UI управляет пользовательским включением.

## Инвариант

После migration пользователь должен получить безопасное стартовое поведение, не требующее постоянной активности плагина в фоне.
