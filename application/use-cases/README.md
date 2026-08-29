# Application Use Cases

В реализации присутствуют use cases для:

- `CollectContext`;
- `BuildContextTree`;
- `BuildParameterIndex`;
- `BuildParameterValues`;
- `BuildQuickFilter`;
- `BuildPreset`;
- `SavePreset` / `DeletePreset` / `ListPresets`;
- инициализации built-in presets.

Use case преобразует пользовательский intent в Domain model и вызывает необходимые порты; он не подменяет собой Revit host boundary.
