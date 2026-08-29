# ViewModel responsibilities

UI использует MVVM. `MainPaneViewModel` является главным UI-оркестратором, но orchestration здесь означает координацию presentation state и вызовов Application, а не владение системной логикой.

## MainPaneViewModel

Координирует пользовательский экран:

```text
collect
filter
actions
presets
history
```

Он связывает specialized ViewModels и async operations, обновляет busy/progress/error state и синхронизирует видимые части экрана.

## Specialized ViewModels

### ContextTreeViewModel / ContextTreeNodeViewModel

Отвечают за представление `Category → Family → Type`, checked state, search/selection controls.

### ParametersViewModel

Отвечает за отображение parameter index, группировку и текущий parameter/value selection.

### QuickFilterViewModel

Отвечает за пользовательский quick-filter interaction и debounced initiation evaluation.

Debounce здесь является поведением взаимодействия, но каноническая компиляция quick filter принадлежит Application.

### ActiveFilterConditionsViewModel

Показывает активные условия пользователю. Он не является владельцем `FilterDefinition`.

### PresetItemViewModel

Представляет preset в списке. Persistence и `PresetDefinition` принадлежат другим слоям.

### ScopeOptionViewModel

Представляет scope option и `BlockedReason`, позволяя UI объяснить недоступность конкретного варианта.

### ParameterGroupNodeViewModel

Организует parameter presentation groups.

## MVVM infrastructure

Реализация содержит:

- `ViewModelBase` / `INotifyPropertyChanged`;
- `RelayCommand`, `AsyncRelayCommand`;
- converters;
- `UiDispatcher` для UI-thread marshaling;
- `WpfApplicationBootstrap` и `MainPaneViewHost`.

## Инвариант

```text
ViewModel coordinates presentation
!= ViewModel owns system semantics
```

Если правило должно оставаться истинным вне WPF, его canonical owner находится не в UI.
