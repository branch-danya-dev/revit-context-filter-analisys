# Consistency Checklist

При изменении системной модели проверить:

## Boundary

- Не приписали ли мы ContextFilter ownership над Revit source state?
- Не смешали ли Domain meaning с Revit API representation?
- Не превратили ли implementation class в новый системный concept без причины?

## Data

- Указан ли canonical owner данных?
- Разделены ли source state, derived state и persisted intent?
- Не трактуется ли cache как authority?
- Разделены ли missing / empty / not-loaded состояния там, где это влияет на смысл?

## Flow

- Понятно ли, где начинается пользовательский intent?
- Понятно ли, где выполняется evaluation?
- Отделён ли FilterResult от последующего action?
- Все Revit write operations пересекают корректную host boundary?

## Lifecycle

- Что происходит при DocumentChanged?
- Что происходит при смене active view / selection?
- Что происходит при закрытии / смене документа?
- Останавливается ли тяжёлая background activity, когда plugin session неактивна?

## Traceability

- Есть ли evidence для утверждения?
- Это исходное требование, аналитический вывод, implementation evidence или post-test correction?
- Не выдаём ли derived acceptance baseline за формально согласованный customer checklist?

## Change surface

Если меняется Domain semantics, перепроверить Application evaluation и Revit/native representation.

Если меняется Revit lifecycle, перепроверить cache freshness, UI reset и background work.

Если меняется persistence schema, перепроверить migration, validation и preset/settings restore flows.
