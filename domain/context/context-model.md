# Collected Context

`CollectedContext` — domain-контракт собранного рабочего контекста.

Из implementation analysis подтверждена структура:

```text
CollectedContext
├─ Scope
├─ ElementIds
├─ CacheKey
├─ TreeRoots
└─ Records
```

`ContextCacheKey` включает признаки document/view/scope/selection/change version, необходимые для различения контекстов.

## Семантическая роль

Контекст отвечает на два разных вопроса:

```text
Какие элементы входят в candidate set?
→ ElementIds

Как этот candidate set представлен для навигации?
→ TreeRoots / Records
```

Физическая ко-локация этих данных в одном DTO не означает, что все downstream-проекции становятся source authority.

## Context identity

Два контекста нельзя считать эквивалентными только потому, что они относятся к одному документу.

На identity могут влиять:

- документ;
- активный вид;
- `CollectionScope`;
- текущее selection;
- версия изменений модели.

## Freshness

`CollectedContext` является **derived state**.

```text
Revit state
→ collection
→ CollectedContext
```

Поэтому сам факт существования объекта не доказывает его актуальность.

Domain хранит identity/provenance, а Application/Revit отвечают за решение `reuse / patch / invalidate / recollect`.

## Инварианты

1. `CollectedContext` не становится authority для Revit-документа.
2. Valid empty context отличается от отсутствующего или устаревшего контекста.
3. Candidate set должен соответствовать заявленному `Scope`.
4. Изменение source context может сделать derived context устаревшим.
5. Навигационное дерево не должно добавлять элементы, отсутствующие в candidate set.
