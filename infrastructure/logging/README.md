# Logging

Infrastructure реализует файловый logging adapter для `IAppLogger`.

## Durable location

```text
%AppData%\ContextFilter\logs\
```

`FileAppLogger` зарегистрирован как реализация Application logging port.

## Ownership

```text
Application / adapters emit diagnostic event
              ↓
           IAppLogger
              ↓
        FileAppLogger
              ↓
          log files
```

Infrastructure отвечает за durable logging mechanism, но не за смысл каждой диагностической метрики.

Например:

- `PerformanceLogger` относится к runtime/application diagnostics;
- `CrashTrace` относится к Revit/WPF stability boundary;
- `FileAppLogger` отвечает за запись логов в файл.

## Инварианты

- лог не является источником Domain truth;
- отсутствие log entry не доказывает отсутствие события;
- logging failure не должен изменять смысл фильтра или Revit authority;
- internal diagnostic detail не должен превращаться в пользовательский success/failure contract без явного mapping.
