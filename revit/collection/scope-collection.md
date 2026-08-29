# Реализация областей сбора в Revit

Domain определяет `CollectionScope`, а слой Revit реализует его через API среды.

## ActiveView

```text
CollectionScope.ActiveView
→ текущий Document + активный View
→ FilteredElementCollector для активного вида
```

## EntireDocument

```text
CollectionScope.EntireDocument
→ текущий Document
→ сбор элементов документа
```

Предупреждение больших моделей и порционный сбор не меняют смысл области.

## CurrentSelection

```text
CollectionScope.CurrentSelection
→ текущее UIDocument.Selection
→ выбранные ElementId
```

Изменение выделения делает прежний контекст `CurrentSelection` неактуальным.

`SupportedElementRules` исключает `View`, `ElementType` и внутренние категории.

```text
смысл области → Domain
способ запросить её в Revit → Revit
```
