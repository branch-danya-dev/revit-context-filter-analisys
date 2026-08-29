# Revit Transactions

Read-only client-side filtering не требует Revit transaction.

Native operations, которые изменяют состояние документа / вида, должны выполняться в допустимой Revit transaction boundary.

Реальный defect обнаружился при isolation: операция выполнялась вне необходимого transaction context и была исправлена.

Принцип:

```text
valid application intent
!= permission to write to Revit outside host transaction rules
```
