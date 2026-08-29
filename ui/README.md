# UI

`ContextFilter.UI` — WPF слой, отвечающий за представление, ViewModels и пользовательский interaction state.

Основная форма — Revit DockablePane.

UI позволяет:

- выбирать scope;
- работать с Category → Family → Type;
- выбирать параметры и значения;
- собирать quick / advanced filter intent;
- видеть matched count;
- запускать selection / visibility / native-filter actions;
- управлять presets и настройками.

UI не определяет Domain semantics и не должен напрямую выполнять Revit API operations.
