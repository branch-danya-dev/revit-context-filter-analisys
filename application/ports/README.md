# Порты Application

Порты задают то, что Application требует от внешнего мира, не фиксируя конкретную реализацию.

- [`revit-gateway.md`](revit-gateway.md) — взаимодействие с Autodesk Revit;
- [`supporting-ports.md`](supporting-ports.md) — хранение, настройки, представление, диалоги и журналирование.

```text
Application зависит от интерфейса
Адаптер зависит от контракта Application
```

Порт не должен раскрывать типы Revit API или детали JSON, если Application не нуждается в них семантически.
