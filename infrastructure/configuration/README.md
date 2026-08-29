# Configuration

Настройки имеют schema version и проходят validation / sanitization при загрузке.

Это появилось как исправление реального дефекта: некорректные пользовательские значения могли приводить к runtime errors.

Ключевой принцип:

```text
persisted configuration
!= automatically trusted runtime state
```
