# Hotkeys and host interaction

`HotkeyService` является Revit-specific integration, потому что shortcut должен учитывать foreground host и native Revit interaction semantics.

## Current source-derived behavior

Hotkeys являются opt-in через `HotkeysEnabled` и по умолчанию выключены.

Подтверждённые combinations:

| Gesture | Behavior |
|---|---|
| `Shift+F` | открыть ContextFilter, если hotkeys включены |
| Double `F` | открыть ContextFilter, если hotkeys включены |
| `Ctrl+Shift+Click` | disabled в текущем source-derived состоянии из-за конфликта с multi-select |

Реализация использует low-level keyboard hooks только когда Revit находится на переднем плане.

## Evolution

Во время тестирования первоначальный `Ctrl+Click` конфликтовал с native Revit multi-selection. Он был заменён на `Ctrl+Shift+Click`, а дальнейшая стабилизация сделала hotkeys opt-in и ограничила конфликтный gesture.

## Системный принцип

> Plugin convenience не должен незаметно красть established host semantics.

```text
plugin shortcut
must coexist with
Revit native selection/input model
```

## Lifecycle

Hooks не должны означать постоянную тяжёлую активность плагина. User-session lifecycle и foreground Revit context ограничивают их полезную область работы.
