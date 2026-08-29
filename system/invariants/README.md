# System Invariants

1. **Revit остаётся authority** для документа, элементов, видов и native state.
2. **Domain semantics не зависят от Revit API representation.**
3. `ParameterKey` важнее display name при идентификации параметра.
4. `missing != empty != not loaded`.
5. Один и тот же `FilterDefinition` должен давать один и тот же matched set независимо от evaluation strategy.
6. `FilterResult` не определяет, какое действие пользователь выполнит над результатом.
7. Semantically valid filter может не иметь эквивалентного native Revit filter.
8. Смена / закрытие документа инвалидирует document-bound state и background work.
9. Когда панель не используется, плагин не должен поддерживать тяжёлую фоновую активность без необходимости.
10. Plugin interaction не должен ломать нативные пользовательские жесты Revit.
