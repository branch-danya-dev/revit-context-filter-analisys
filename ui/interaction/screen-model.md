# Screen model

Основная форма `MainPaneView` представляет одну filter session через несколько связанных зон.

## Верхняя панель

Показывает состояние текущей сессии и быстрые controls:

- заголовок;
- dynamic highlight toggle;
- matched element count;
- refresh;
- progress.

Эти элементы отображают или инициируют состояние, но не являются источником истины для Domain/Application.

## Filter workspace

```text
Scope
↓
Actions
↓
┌─────────────────────┬─────────────────────┬─────────────────────┐
│ Category / Family / │ Parameters          │ Values / result     │
│ Type                │                     │                     │
└─────────────────────┴─────────────────────┴─────────────────────┘
↓
Presets / history
```

### 1. Scope

Пользователь выбирает `ActiveView`, `EntireDocument` или `CurrentSelection`.

UI показывает доступность option и причину блокировки, но канонический смысл `CollectionScope` принадлежит Domain, а проверка/сбор контекста — Application/Revit.

### 2. Actions

Пользователь может инициировать:

- Replace selection;
- Add to selection;
- Exclude from selection;
- Hide;
- Isolate;
- inverse isolate;
- reset temporary visibility;
- create/replace native Revit filter.

### 3. Category / Family / Type

TreeView является навигационной проекцией текущего context. Отмеченные узлы — UI selection state, а не Revit selection.

### 4. Parameters

Параметры отображаются как доступные пользователю labels/groups. Display name не заменяет канонический `ParameterKey`.

### 5. Values / result

Показываются значения выбранного parameter и активные filter conditions. Видимый список значений — derived projection текущего context, а не самостоятельная persisted truth.

### 6. Presets / history

UI позволяет загрузить, сохранить или удалить preset и повторно использовать history. Persisted representation принадлежит Infrastructure, а `PresetDefinition` — Domain.

## Другие tabs

Реализация также содержит Help и Settings tabs. Они относятся к presentation/configuration interaction и не создают отдельной системной ответственности.
