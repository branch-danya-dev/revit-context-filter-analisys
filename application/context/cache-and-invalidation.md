# Cache and invalidation

Application использует многоуровневый `ContextCollectionCache`.

Подтверждённая зависимость уровней:

```text
IDs
↓
Tree
↓
Index + Snapshots
↓
FilterResult
```

| Tier | Содержимое | Инвалидация |
|---|---|---|
| IDs | `ElementIds` + cache key | view / selection / model change |
| Tree | `ContextNode[]` | при invalidation IDs |
| Index | parameter index + snapshots | при invalidation Tree или значительном model change |
| Filter | `FilterResult` | при invalidation Index или изменении `FilterDefinition` |

## Смысл

Кэш хранит derived state и ускоряет повторные вычисления. Он не меняет ownership данных.

```text
cache hit
!= source authority
```

## Responsibility split

- Application определяет зависимость derived tiers;
- Revit сообщает host lifecycle/model changes и обеспечивает source recollection;
- UI решает, как показывать stale/progress state.

Конкретные thresholds и Idling/chunking относятся к runtime/Revit realization и не являются семантикой этого документа.