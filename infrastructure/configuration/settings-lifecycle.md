# Settings lifecycle

Settings проходят несколько разных состояний.

```text
ABSENT
  ↓ defaults
RUNTIME SETTINGS

PERSISTED
  ↓ deserialize
LOADED
  ↓ migrate
CURRENT SCHEMA
  ↓ sanitize
TRUSTED RUNTIME SETTINGS
```

## Почему это важно

В пользовательском тестировании некорректные сохранённые настройки могли приводить к ошибкам. После этого в систему была добавлена автоматическая validation/normalization граница.

Поэтому загрузка settings должна пониматься не как:

```text
JSON → object → use
```

а как:

```text
JSON
→ parse
→ schema handling
→ validation / normalization
→ use
```

## Инварианты

- persisted settings не являются authority для Revit state;
- invalid persisted value не должен без проверки управлять тяжёлой runtime operation;
- defaults должны давать работоспособное состояние при отсутствии пользовательской конфигурации;
- изменение settings schema не должно требовать ручного редактирования файлов пользователем.

## Связи

Конкретное применение настроек в UI/Application/Revit описывается соответствующими владельцами. Здесь каноничен только lifecycle persisted configuration.
