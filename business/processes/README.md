# User Processes

Этот раздел описывает ContextFilter как пользовательский workflow, не раскрывая внутреннюю Application orchestration.

## Основной сценарий

- [`main-user-journey.md`](main-user-journey.md) — полный путь от рабочего контекста Revit до действия над matched set;
- [`diagrams/main-user-flow.puml`](diagrams/main-user-flow.puml) — визуальный flow.

Коротко:

```text
Работа в Revit
→ выбрать scope
→ открыть ContextFilter
→ Category / Family / Type
→ parameter + value
→ filter result
→ action
→ продолжить работу в Revit
```

Альтернативный путь начинается с предварительного selection и ограничивает дальнейшую фильтрацию этим набором.

Сохранение preset / template является дополнительным шагом повторного использования filter intent, а не обязательной частью каждого сценария.

Техническая orchestration этих действий принадлежит [`../../application/`](../../application/).
