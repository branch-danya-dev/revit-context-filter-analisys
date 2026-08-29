# Collection Scope

`CollectionScope` определяет **семантическую границу candidate set**, внутри которой пользователь формулирует фильтр.

Поддерживаются три значения:

```text
ActiveView
EntireDocument
CurrentSelection
```

## ActiveView

Candidate set ограничен элементами текущего активного вида.

Смысл:

> Найти элементы в рабочем контексте, который пользователь сейчас видит.

## EntireDocument

Candidate set относится ко всему текущему Revit-документу.

Смысл:

> Найти элементы независимо от их присутствия на активном виде.

Предупреждения о размере модели и техническая стратегия collection не являются частью domain semantics этого scope.

## CurrentSelection

Candidate set ограничен текущим пользовательским выделением.

Смысл:

> Использовать уже сформированный вручную набор как входную область дальнейшей фильтрации.

## Инварианты

1. `CollectionScope` определяет universe поиска, а не сам filter condition.
2. Один и тот же `FilterDefinition` при разных scope может давать разные matched sets.
3. `CurrentSelection` не означает "все элементы документа, соответствующие выбранным" — universe ограничен исходным selection.
4. Изменение scope меняет контекст фильтрации и требует повторного получения соответствующего candidate set.
5. Performance policy не должна менять семантическое значение scope.

## Связи

`CollectionScope` входит в:

- `FilterDefinition`;
- `CollectedContext`;
- `PresetDefinition`.

Это позволяет filter intent явно сохранять, **где** он должен применяться.
