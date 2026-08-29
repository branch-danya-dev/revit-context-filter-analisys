# Infrastructure

`Infrastructure` содержит адаптеры для локального хранения, конфигурации и файлового логирования.

Он реализует порты, объявленные в `application/`, но не владеет смыслом фильтров, пресетов или пользовательских сценариев.

## Ответственность

```text
Application ports
      ↓
Infrastructure adapters
      ↓
%AppData%\ContextFilter
```

Infrastructure отвечает за:

- JSON persistence;
- физическое расположение локальных данных;
- migration persisted schemas;
- validation / sanitization загруженной конфигурации;
- atomic file replacement для критичных записей;
- deduplication и ограничение истории;
- файловое логирование;
- регистрацию собственных adapter implementations в DI.

Infrastructure **не** отвечает за:

- Domain semantics;
- orchestration use cases;
- Revit API access;
- WPF state;
- выбор runtime evaluation strategy.

## Структура

```text
infrastructure/
├─ persistence/     → JSON stores, layout, atomic writes, schema migration
├─ configuration/   → settings lifecycle, sanitization, calm defaults
├─ logging/         → file logging boundary
└─ diagrams/        → infrastructure data flow
```

## Главная граница доверия

```text
file exists
!= file is valid runtime state
```

Persisted data является внешним входом для приложения. После чтения оно должно пройти применимые migration и validation/sanitization шаги прежде, чем станет доверенным runtime state.

## Связи

- [`../application/ports/`](../application/ports/) — контракты, которые реализует Infrastructure;
- [`../domain/presets/`](../domain/presets/) — semantic model пресета;
- [`persistence/`](persistence/) — stores и durable data;
- [`configuration/`](configuration/) — настройки и граница доверия;
- [`logging/`](logging/) — локальная диагностика.
