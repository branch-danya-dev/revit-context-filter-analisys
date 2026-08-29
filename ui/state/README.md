# UI State

UI state привязан к текущей Revit session / document context там, где данные зависят от документа.

После тестирования было добавлено автоматическое очищение document-specific UI state при смене документа.

```text
document changes
→ old document-bound UI state is no longer valid
→ reset / rebuild from new context
```
