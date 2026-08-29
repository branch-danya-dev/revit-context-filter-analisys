# Scope collection

Domain определяет три `CollectionScope`, а Revit layer реализует их через host API.

## ActiveView

```text
CollectionScope.ActiveView
→ current Document + active View
→ FilteredElementCollector scoped to active view
```

Смена active view меняет host boundary такого context и требует пересмотра derived data.

## EntireDocument

```text
CollectionScope.EntireDocument
→ current Document
→ project-wide element collection
```

Это самый широкий host read. Предупреждение для больших моделей и chunking являются runtime guardrails, а не изменением смысла scope.

## CurrentSelection

```text
CollectionScope.CurrentSelection
→ UIDocument current selection
→ selected ElementIds
```

Selection является частью host state. Если пользователь меняет selection, ранее собранный CurrentSelection context больше не описывает текущий selection.

## Supported element rules

Implementation analysis подтверждает исключение:

- `View`;
- `ElementType`;
- internal categories.

Эти правила принадлежат Revit adapter, потому что выражают, какие Autodesk Revit entities могут быть представлены ContextFilter как рабочие project elements.

## Ключевое различие

```text
scope meaning
→ Domain

how to query Revit for that scope
→ Revit
```
