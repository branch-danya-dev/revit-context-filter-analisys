# Traceability

Этот раздел связывает исходную потребность с нормализованными требованиями, владельцами технического знания и фактической проверкой.

## Канонический документ

- [`traceability-matrix.md`](traceability-matrix.md) — requirement → source → canonical owner → validation evidence.

## Цепочка

```text
Customer need
→ normalized requirement
→ system responsibility
→ local technical owner
→ implementation
→ user testing
→ accepted behavior
```

## Evidence

Используются:

- два первоначальных ТЗ;
- уточнения в общении с заказчиком и BIM-координатором;
- фактически реализованная система;
- результаты пользовательского тестирования и stabilization fixes;
- финальная приёмка директором;
- факт внедрения.

Матрица не становится вторым источником истины для Domain / Application / Revit behavior. Она только показывает, где находится каноническое описание и почему оно существует.
