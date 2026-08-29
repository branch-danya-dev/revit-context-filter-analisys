# User Process

Базовый пользовательский сценарий:

```text
Открыть ContextFilter из Ribbon
        ↓
Выбрать scope
        ↓
Получить Category → Family → Type
        ↓
Сузить набор категорий / типов
        ↓
Выбрать параметр и значения
        ↓
Сформировать FilterDefinition
        ↓
Получить matched elements
        ↓
Выбрать действие
        ↓
Selection / Hide / Isolate / Native Filter
```

Дополнительно пользователь может сохранить filter intent как preset или template и повторно применить его в другом рабочем контексте.
