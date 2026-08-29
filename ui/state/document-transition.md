# Document transition

Часть UI state зависит от текущего Revit document и должна инвалидироваться вместе с ним.

## Problem

Во время пользовательского тестирования UI state мог пережить переключение документа. Это создавало ложную видимость того, что старые tree/parameters/results относятся к новому document context.

## Corrected behavior

```text
Document A active
→ UI state derived from A

switch / close A
→ invalidate A-bound UI state
→ cancel/ignore obsolete pending responses
→ reset document-specific presentation

Document B active
→ collect/rebuild only for B
```

## Что считается document-bound

К document-bound presentation относятся, как минимум, проекции текущих:

- context tree;
- parameter/index values;
- matched result/count;
- current selection-derived scope presentation;
- progress/result, относящиеся к obsolete request.

## Что не обязано исчезать

Не вся UI state обязана быть document-specific. Например, user preferences или reusable preset intent могут существовать независимо от текущего документа.

```text
document switch
!= delete persisted settings
!= delete presets
```

## Инвариант

> UI не должен показывать derived state как текущий, если document identity, из которого он был получен, больше не является текущим.

Механизм обнаружения `DocumentOpened/Closed/Changed` и host cancellation принадлежит `revit/`; UI владеет очисткой и новым presentation state после такого сигнала.
