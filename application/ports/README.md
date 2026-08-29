# Application ports

Ports задают то, что Application требует от внешнего мира, не фиксируя конкретную реализацию.

## Основные группы

- [`revit-gateway.md`](revit-gateway.md) — взаимодействие с host application;
- [`supporting-ports.md`](supporting-ports.md) — persistence, settings, presentation, dialogs, logging.

## Правило Ports & Adapters

```text
Application depends on interface
Adapter depends on Application contract
```

Порт не должен раскрывать Revit API types или JSON-specific details, если Application не нуждается в них семантически.