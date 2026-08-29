# System Flows

## Открытие и сбор контекста

```text
Ribbon command
→ UI session starts
→ Application requests context
→ IRevitGateway
→ ExternalEvent
→ Revit collector
→ ElementIds + tree records
→ UI binds context
```

## Фильтрация

```text
User selects parameter values
→ Quick Filter use case
→ FilterDefinition
→ FilterEvaluator
→ matched ElementIds
→ UI shows count / optional highlight
```

## Действие

```text
Matched ElementIds
→ action intent
→ Application calculation / validation
→ Revit gateway
→ ExternalEvent
→ native Revit operation
```

Смысл фильтра и действие над результатом — разные стадии.
