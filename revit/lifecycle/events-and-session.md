# Events and user session

`ContextCoordinationService` связывает Revit host events с текущим состоянием ContextFilter.

## Подтверждённые responsibilities

- debounced `DocumentChanged`;
- incremental patch vs full invalidate;
- opt-in auto-refresh при смене view;
- progress chunked collection;
- session lifecycle gating;
- selection-change handling для `CurrentSelection`.

## DocumentChanged

```text
DocumentChanged
↓
debounce
↓
known bounded change?
├─ yes → attempt incremental patch
└─ no  → mark stale / invalidate
```

Точные thresholds принадлежат runtime configuration и не являются смыслом Revit event.

## Idling

При `IsUserSessionActive == false` implementation analysis подтверждает:

- no collect;
- no index work;
- no highlight work.

При активной session `Idling` может:

- pump chunked collection;
- заметить selection change;
- инициировать opt-in auto-refresh.

## Document transition

Document close/switch должен приводить к прекращению obsolete document-bound work и обновлению downstream UI/context state.

## Инвариант

```text
host event received
!= always perform expensive work
```

Event является evidence об изменении host state. Конкретная реакция зависит от active session, scope и freshness policy.
