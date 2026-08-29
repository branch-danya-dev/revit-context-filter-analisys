# Revit Lifecycle

Plugin state зависит от lifecycle активного Revit документа, вида и selection.

Реализация отслеживает:

- document changes;
- active view changes;
- selection changes;
- document close / switch;
- active plugin session;
- Revit shutdown.

После пользовательского тестирования background handlers были ограничены активной plugin session, document-bound work останавливается после закрытия документа, а shutdown освобождает ресурсы без прежних задержек.
