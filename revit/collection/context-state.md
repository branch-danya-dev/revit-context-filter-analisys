# Revit context state

`RevitContextState` отслеживает host identity, от которой зависит актуальность собранного context.

Подтверждённые dimensions:

- current document identity;
- active view tracking;
- current selection hash.

## Почему это Revit responsibility

Application может хранить derived cache key, но только Revit adapter может наблюдать фактический host state, который делает этот key актуальным или устаревшим.

```text
Revit Document / View / Selection
↓
RevitContextState
↓
change evidence
↓
Application cache / UI refresh decisions
```

## Host transitions

### View change

Имеет значение прежде всего для `ActiveView` scope.

### Selection change

Имеет значение для `CurrentSelection` scope.

### Model change

`DocumentChanged` может потребовать incremental patch либо invalidation/recollect.

### Document switch/close

Старый document-bound context больше не должен считаться допустимым источником для новой пользовательской сессии.

## Инвариант

```text
same in-memory object
!= same valid Revit context
```

Валидность определяется совместимостью derived state с текущим host identity, а не тем, что объект всё ещё находится в памяти.
