# Domain

`ContextFilter.Domain` хранит **канонический смысл предметной модели плагина**, не зависящий от WPF, JSON persistence или Autodesk Revit API.

Основные области:

- [`context/`](context/) — scope и элементные snapshots;
- [`parameters/`](parameters/) — identity и typed values;
- [`filtering/`](filtering/) — FilterDefinition, logical tree и operators;
- [`presets/`](presets/) — reusable filter intent;
- [`results/`](results/) — derived filter result.

Domain не владеет Revit-транзакциями, UI state, файловым хранением или алгоритмом ExternalEvent.
