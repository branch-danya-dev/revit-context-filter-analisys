# Transactions

`transactions/` описывает границу записи в Autodesk Revit.

Не каждая операция ContextFilter изменяет model/view state, и не каждая host operation имеет одинаковые transaction requirements. Поэтому transaction нельзя считать глобальной оболочкой всего filter workflow.

## Базовая модель

```text
read-only collect / evaluate
→ no semantic reason to open write transaction

host mutation that requires Revit transaction
→ execute inside valid transaction context
```

Client-side filter evaluation выполняется на `ElementSnapshot` и не требует Revit transaction.

Selection работает через `UIDocument.Selection`; visibility/native-filter operations имеют свои host rules.

## Канонический документ

- [`transaction-boundary.md`](transaction-boundary.md)

## Реальный validation evidence

Во время пользовательского тестирования isolation выполнялась вне требуемого transaction context и вызывала ошибку. Исправление подтвердило, что transaction correctness — часть Revit adapter design, а не деталь UI.

## Инвариант

> Domain/Application correctness не освобождает Revit adapter от соблюдения host mutation contract.
