# Diagnostic boundary

ContextFilter имеет несколько видов диагностических механизмов, но Infrastructure владеет только durable logging adapter.

## Разделение

```text
runtime metric / error context
→ diagnostic producer
→ IAppLogger
→ FileAppLogger
→ file
```

### Infrastructure

- `FileAppLogger`;
- log file location;
- durable log write mechanics.

### Application / runtime

- performance meaning;
- operation duration / element-count measurements;
- decisions о том, какие события стоит измерять.

### Revit

- host crash trace;
- Revit lifecycle context;
- host-specific exception boundaries.

## Почему это важно

Наличие общей папки `logs/` не делает Infrastructure владельцем всех failure semantics.

```text
where evidence is stored
!= who owns meaning of evidence
```

## Граница evidence

Source analysis подтверждает `FileAppLogger`, `PerformanceLogger` и `CrashTrace`, но не задаёт публичный logging schema или retention policy. Поэтому они здесь не придумываются.
