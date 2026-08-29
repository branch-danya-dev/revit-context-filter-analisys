# Storage layout

ContextFilter хранит собственные локальные данные в пользовательском профиле:

```text
%AppData%\ContextFilter\
├─ settings.json
├─ presets.json
├─ recent.json
└─ logs\
```

## Назначение файлов

### `settings.json`

Хранит пользовательские и runtime-настройки плагина, включая настройки поведения UI и производительности.

### `presets.json`

Хранит пользовательские и встроенные пресеты. Semantic model пресета определена в `domain/presets/`; Infrastructure отвечает только за durable representation и migration.

### `recent.json`

Хранит историю фильтров. Infrastructure применяет deduplication по signature и ограничение количества элементов через `HistoryMaxItems`.

### `logs/`

Каталог файловых логов `FileAppLogger`.

## Ownership

```text
Domain object / Application state
        ↓
Infrastructure serialization
        ↓
local file
```

Файл не становится владельцем смысла объекта только потому, что является durable copy.

## Инварианты

- удаление cache-like runtime data не должно менять Domain meaning;
- corrupt/old file не должен напрямую становиться runtime state;
- путь хранения является Infrastructure concern, а не частью Domain contract;
- Revit document не хранится и не реплицируется в этих файлах.
