# Product Scope

Этот раздел отвечает на вопрос:

> **Что именно является ContextFilter, а что остаётся ответственностью Revit или вообще другой задачи?**

## Канонический документ

- [`scope.md`](scope.md) — in-scope / out-of-scope, граница с Autodesk Revit и правило изменения scope;
- [`diagrams/product-boundary.puml`](diagrams/product-boundary.puml) — визуальная граница продукта.

## В системе

```text
Working context
→ element discovery
→ parameter filtering
→ reusable presets
→ actions on matched set
```

В продукт входят Active View / Entire Document / Current Selection, Category → Family → Type, параметрическая фильтрация, selection / visibility actions, compatible native Revit filter и reusable presets.

## Вне границы

ContextFilter не является редактором BIM-модели и не владеет исходными данными Revit.

В частности, отдельная задача по корректировке DWG не включается в этот кейс без подтверждённой связи с ContextFilter.

Подробности: [`scope.md`](scope.md).
