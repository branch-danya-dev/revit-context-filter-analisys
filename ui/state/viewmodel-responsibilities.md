# Ответственности ViewModel

UI использует MVVM. `MainPaneViewModel` является главным координатором экрана, но эта координация относится к состоянию представления и вызовам Application, а не к владению системной логикой.

## MainPaneViewModel

Связывает сбор, фильтрацию, действия, пресеты и историю, координирует специализированные ViewModel и асинхронные операции, обновляет прогресс и ошибки.

## Специализированные ViewModel

- `ContextTreeViewModel` / `ContextTreeNodeViewModel` — представление Категория → Семейство → Тип, отметки, поиск;
- `ParametersViewModel` — индекс параметров, группировка и выбор параметра/значения;
- `QuickFilterViewModel` — взаимодействие быстрого фильтра и отложенный запуск вычисления;
- `ActiveFilterConditionsViewModel` — отображение активных условий;
- `PresetItemViewModel` — элемент списка пресетов;
- `ScopeOptionViewModel` — вариант области и `BlockedReason`;
- `ParameterGroupNodeViewModel` — группы параметров.

Задержка UI-событий не меняет каноническую компиляцию быстрого фильтра, которая принадлежит Application.

## Инфраструктура MVVM

Реализация содержит `ViewModelBase` / `INotifyPropertyChanged`, `RelayCommand`, `AsyncRelayCommand`, converters, `UiDispatcher`, `WpfApplicationBootstrap`, `MainPaneViewHost`.

```text
ViewModel координирует представление
!= ViewModel владеет системной семантикой
```

Если правило должно оставаться истинным вне WPF, его канонический владелец находится не в UI.
