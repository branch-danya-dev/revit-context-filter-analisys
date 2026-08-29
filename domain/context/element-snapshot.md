# ElementSnapshot

`ElementSnapshot` — представление одного Revit-элемента в Domain для клиентской фильтрации без постоянного обращения к Revit API.

```text
ElementSnapshot
├─ ElementId
├─ UniqueId
├─ CategoryIdentity
├─ FamilyTypeIdentity
├─ Parameters: ParameterKey → ParameterValue
└─ ParameterDisplayNames: ParameterKey → string
```

```text
Revit Element
!=
ElementSnapshot
```

Revit `Element` остаётся объектом среды. `ElementSnapshot` — производное представление необходимых данных.

Параметры ищутся по `ParameterKey`, а отображаемое имя хранится отдельно.

> **Читаемая подпись не является канонической идентичностью параметра.**

Реализация использует лёгкие снимки и последующую отложенную загрузку параметров. Поэтому:

```text
параметр ещё не загружен в текущий снимок
!=
доказано, что параметр отсутствует у элемента Revit
```

Неполнота технического представления не должна превращаться в смысл `NotExists`.
