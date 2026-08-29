# System Architecture

Реализация использует Clean Architecture.

```text
UI ───────────────→ Application ─────→ Domain
                         ↑
Infrastructure ──────────┘

Revit ─────────────→ UI / Application / Domain / Infrastructure
  │
  └─ Autodesk Revit API
```

## Ответственности

| Зона | Ответственность |
|---|---|
| Domain | Семантика фильтра, parameter identity, snapshots, presets |
| Application | Use cases, evaluation, orchestration, ports |
| Infrastructure | JSON persistence, settings, logging, DI support |
| UI | WPF views, view models, interaction state |
| Revit | Add-in entry, ExternalEvent, collectors, native actions, events, transactions |

## Правило зависимостей

Domain не зависит от Revit API. Application работает через абстракции. Revit является host-specific realization внешней границы.
