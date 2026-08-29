# Domain

Этот раздел хранит **семантическое ядро ContextFilter** — модели и понятия, смысл которых не должен зависеть от WPF, Revit API, JSON persistence или конкретного алгоритма вычисления.

Domain отвечает на вопрос:

> **Что означает контекст, параметр, фильтр, пресет и результат независимо от способа их технической реализации?**

## Структура

- [`context/`](context/) — область сбора, domain-контекст, дерево Category → Family → Type и `ElementSnapshot`;
- [`parameters/`](parameters/) — идентичность параметров, значения и synthetic properties;
- [`filtering/`](filtering/) — `FilterDefinition`, дерево условий, логические группы и операторы;
- [`presets/`](presets/) — reusable filter intent: Full и Template;
- [`results/`](results/) — результат фильтрации и типы действий над найденным набором;
- [`diagrams/domain-model.puml`](diagrams/domain-model.puml) — карта основных domain-сущностей.

## Что намеренно не описывается здесь

Domain не владеет:

- сбором элементов через `FilteredElementCollector`;
- `ExternalEvent` и Revit API thread boundary;
- кэшированием, debounce, chunking и parallel evaluation;
- WPF state и presentation logic;
- JSON-файлами настроек и пресетов;
- Revit transactions и созданием `ParameterFilterElement`.

Эти знания принадлежат соответствующим техническим владельцам.

## Dependency rule

`ContextFilter.Domain` не имеет внешних NuGet-зависимостей. Остальные слои могут зависеть от Domain, но Domain не должен знать о них.

```text
UI ───────────────┐
Infrastructure ───┼──→ Application ───→ Domain
Revit ────────────┴───────────────────→ Domain

Domain → ∅
```

## Канонический принцип

```text
Technical representation
!=
Domain meaning
```

Например:

- display name параметра не является его identity;
- `ElementSnapshot` не является Revit `Element`;
- native Revit filter не является каноническим смыслом `FilterDefinition`;
- JSON preset не является владельцем смысла `PresetDefinition`.
