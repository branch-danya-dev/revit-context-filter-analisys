# Action orchestration

После evaluation Application связывает semantic result с выбранным пользователем действием.

```text
FilterResult
        ↓
action intent
        ↓
optional set calculation / compatibility check
        ↓
IRevitGateway
        ↓
response
```

Подтверждённые output operations через `IRevitGateway`:

- apply selection;
- apply temporary visibility;
- apply native filter.

Для native filter перед side effect выполняется compatibility analysis.

## Failure boundary

Успешный `FilterResult` не гарантирует успешный host action. Ошибка, несовместимость native filter или host restriction должны возвращаться как action outcome, а не менять задним числом смысл matched set.

```text
filter success
!= action success
```