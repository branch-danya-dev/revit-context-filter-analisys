# ContextFilter — System Analysis

[RU](README.md) · [EN](README_EN.md)

> A real system-analysis, solution-design, implementation and deployment case for an Autodesk Revit 2025 contextual filtering add-in.

## Project context

| | |
|---|---|
| **My role** | System Analyst · Solution Designer · Developer |
| **Customer** | Deputy director of a design institute |
| **Domain expert** | BIM coordinator |
| **Request** | Build an extended Bimstep / ModPlus-style contextual filtering tool for Revit |
| **Acceptance** | Final result accepted by the institute director |
| **Outcome** | Deployed and in use; no errors or complaints were reported after deployment |

The organization name is intentionally not published.

## System idea

Users work inside an explicit Revit context — active view, entire document or current selection — then narrow elements by category, family, type and parameters and apply the resulting set back to Revit.

```text
Revit document
      ↓
Collection scope
      ↓
Context elements
      ↓
Filter definition
      ↓
Matched element set
      ↓
Selection / visibility / native Revit filter
```

## Implemented architecture

The delivered solution uses Clean Architecture with five projects:

```text
                 ContextFilter.UI
                       │
                       ▼
              ContextFilter.Application
                       │
                       ▼
                 ContextFilter.Domain

ContextFilter.Infrastructure ──→ Application + Domain
ContextFilter.Revit ───────────→ all layers + Revit API
```

This repository follows the same knowledge-architecture principle as [Aveli System Analysis](https://github.com/branch-danya-dev/aveli-system-analysis): business and system-wide knowledge first, then the actual technical ownership areas.

```text
business/
system/
domain/
application/
infrastructure/
ui/
revit/
```

The structure is guided by [SSAD](https://github.com/branch-danya-dev/ssad-methodology), but mirrors the real ContextFilter architecture rather than introducing a parallel taxonomy.
