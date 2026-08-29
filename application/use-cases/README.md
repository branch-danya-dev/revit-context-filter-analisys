# Use cases

Use cases — входные точки Application для законченных пользовательских намерений.

Они связывают Domain-модели, Application services и внешние порты, но не содержат Revit API или WPF-specific behavior.

## Карта

- [`use-case-catalog.md`](use-case-catalog.md) — подтверждённый набор use cases;
- [`quick-filter-compilation.md`](quick-filter-compilation.md) — преобразование UI-выбора в canonical `FilterDefinition`;
- [`preset-lifecycle.md`](preset-lifecycle.md) — построение, сохранение, чтение и инициализация presets.

## Правило

```text
user intent
→ use case
→ domain/application operation
→ result
```

Use case не является владельцем данных, которые он использует. Например, `BuildQuickFilterUseCase` создаёт `FilterDefinition`, но смысл этой модели принадлежит Domain.