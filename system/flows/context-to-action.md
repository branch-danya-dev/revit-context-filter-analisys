# Context → Filter → Action

Это основной end-to-end flow системы.

## 1. Запуск

Пользователь открывает ContextFilter из Revit.

UI запрашивает актуальную информацию о host context через application/Revit boundary.

## 2. Выбор scope

Пользователь выбирает один из поддерживаемых scopes:

- Active View;
- Entire Document;
- Current Selection.

Scope задаёт universe кандидатов, но сам по себе ещё не является фильтром.

## 3. Collection

```text
Application request
→ Revit collector
→ supported element rules
→ element ids / tree records
→ ElementSnapshot representation
```

Большие наборы могут собираться порционно через Idling, но это optimization strategy и не меняет смысл результата.

## 4. Представление контекста

Из собранного набора формируются:

- Category → Family → Type tree;
- parameter index;
- доступные значения параметров;
- synthetic properties.

Это derived representations текущего Revit context.

## 5. Формирование фильтра

Пользовательский выбор преобразуется в `FilterDefinition`.

Quick Filter не является отдельным filter language — это способ собрать canonical domain definition.

## 6. Evaluation

`FilterEvaluator` вычисляет `FilterResult` на in-memory snapshots.

```text
FilterDefinition
+
ElementSnapshot[]
→ FilterEvaluator
→ matched ElementIds
```

Evaluation не требует Revit transaction.

## 7. Action

После получения matched set пользователь выбирает действие.

Application вычисляет требуемый set behavior, а Revit layer реализует host-specific effect:

```text
Replace / Add / Exclude
→ UIDocument.Selection

Hide / Isolate / Inverse / Reset
→ Revit view temporary visibility

Native Filter
→ compatibility check
→ ElementFilter conversion
→ ParameterFilterElement
```

## Инвариант потока

> **FilterResult описывает найденный набор. Action описывает, что пользователь хочет сделать с этим набором. Это разные системные понятия.**
