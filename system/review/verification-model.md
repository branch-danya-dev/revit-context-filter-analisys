# Verification Model

## Источники проверки

Для ContextFilter доступны четыре типа evidence:

1. первоначальные ТЗ заказчика;
2. уточнения требований с заказчиком и BIM-координатором;
3. реализованная архитектура, описанная в `PROJECT_ANALYSIS.md`;
4. пользовательское тестирование, стабилизация, финальная приёмка директором и внедрение.

## Verification chain

```text
Requirement
→ expected system behavior
→ canonical owner
→ implementation representation
→ observed result
```

## Пример: фильтрация по типу

Исходное требование отдельно отмечало недопустимое поведение, при котором условие по типу захватывает элементы других типов.

Verification требует проверить не только UI:

```text
business requirement
→ Domain parameter/filter semantics
→ Application evaluation
→ Revit-collected snapshot values
→ resulting ElementIds
```

## Пример: presets

```text
requirement: reusable presets/templates
→ Domain PresetDefinition
→ Application build/list/save use cases
→ Infrastructure JSON store
→ UI restore workflow
→ repeat evaluation in current Revit context
```

## Пример: lifecycle

Lifecycle corrections появились после тестирования, поэтому source здесь другой:

```text
observed production-like behavior
→ system invariant
→ Revit/Application lifecycle correction
→ repeated user testing
```

## Ограничение публичной проверки

Публичный analysis repository не содержит исходный код production-плагина. Поэтому он может показать traceability между переданными материалами и документированной реализацией, но не должен выдавать документационную проверку за независимый source-code audit.
