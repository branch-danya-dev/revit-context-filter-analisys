# Data Ownership and Movement

## 1. Revit source state

Authority: Autodesk Revit.

Сюда относятся:

- Document;
- Element / ElementId;
- active View;
- selection;
- native parameters;
- native `ParameterFilterElement`;
- host transaction / event state.

ContextFilter может читать или через разрешённые API изменять отдельные части host state, но не становится их источником истины.

## 2. Domain semantic state

Authority для смысла: Domain.

Ключевые модели:

- `FilterDefinition`;
- `FilterGroup` / `FilterCondition`;
- `ParameterKey`;
- `ElementSnapshot` contract;
- `FilterResult`;
- `PresetDefinition`.

Эти модели не обязаны иметь прямой one-to-one аналог в Revit API.

## 3. Derived runtime state

Вычисляется из Revit source state и Domain/Application rules:

- candidate ElementIds;
- Category → Family → Type tree;
- parameter index;
- parameter values;
- inverted indexes;
- cached snapshots;
- current FilterResult.

```text
Revit source state
→ derive
→ runtime projection
```

Derived state может быть отброшен и пересобран. Поэтому cache не является source authority.

## 4. UI state

UI хранит пользовательскую рабочую сессию:

- выбранный scope;
- отмеченные tree nodes;
- выбранный parameter/value;
- видимость панели;
- progress / feedback state;
- локальное состояние interaction.

UI state может отражать Domain/Application state, но не определяет Revit truth.

## 5. Persisted plugin state

Хранится в `%AppData%\ContextFilter`:

```text
settings.json
presets.json
recent.json
logs/
```

Ownership:

- settings → Infrastructure persistence of plugin configuration;
- presets → persisted reusable user intent;
- recent → filter history;
- logs → diagnostic evidence.

Persisted configuration проходит validation / normalization перед использованием. Файл на диске не считается автоматически доверенным runtime state.

## Движение данных

```text
REVIT
  ↓ collect
DOMAIN REPRESENTATIONS
  ↓ application processing
DERIVED RUNTIME STATE
  ↓ presentation
UI

UI intent
  ↓
DOMAIN / APPLICATION
  ↓ optional persistence
INFRASTRUCTURE

FilterResult
  ↓ action request
REVIT
```

## Главные различия

```text
Element != ElementSnapshot
Parameter label != ParameterKey
Preset != FilterResult
Cache != Revit authority
Persisted setting != validated runtime setting
```
