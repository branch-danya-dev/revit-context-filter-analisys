# Selection and visibility calculation

Application содержит два подтверждённых вычислительных сервиса:

- `SelectionSetCalculator` — поддерживает selection intents `Replace`, `Add`, `Exclude`;
- `VisibilitySetCalculator` — вычисляет наборы для visibility scenarios, включая inverse isolation.

Их задача — отделить set logic от host API execution.

```text
current host-derived state
+ matched element set
+ action intent
        ↓
Application calculator
        ↓
target element set
```

## Почему это важно

Revit adapter не должен самостоятельно придумывать business/application semantics действия. Он получает уже определённый intent/target set и реализует его средствами host API.

Точные host restrictions — например, какие элементы реально могут быть скрыты на конкретном view — принадлежат Revit layer и могут скорректировать/отклонить выполнение.