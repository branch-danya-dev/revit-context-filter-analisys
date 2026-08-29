# Host interaction

ContextFilter lives inside Autodesk Revit, поэтому UI gestures должны сосуществовать с native interaction model host-приложения.

## Launch

В финальном delivery flow плагин не должен автоматически открывать тяжёлую UI session при старте Revit. Основной запуск выполняется через Ribbon.

## Hotkeys

Hotkeys являются opt-in feature.

История тестирования:

```text
Ctrl + Click
→ conflict with Revit multi-selection
→ replaced during testing with Ctrl + Shift + Click
→ later source state constrains/disables this gesture
```

В source-derived финальном состоянии:

- `Shift+F` может открывать фильтр при включённых hotkeys;
- Double `F` поддерживается при включённых hotkeys;
- `Ctrl+Shift+Click` отмечен как disabled из-за конфликта с multi-select.

Поэтому каноническое правило важнее конкретной клавиши:

> Plugin shortcut must not steal or ambiguously redefine established Revit selection semantics.

## Calm interaction defaults

В schema v2 по умолчанию выключены:

- auto-refresh;
- dynamic highlight;
- hotkeys.

Это отражает правило:

```text
convenience
must not imply
continuous host activity
```

## Ownership

UI владеет тем, как возможность представлена пользователю и включена/выключена. Low-level keyboard hooks, foreground detection и host event mechanics принадлежат `revit/`.
