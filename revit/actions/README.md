# Revit Actions

Host-specific actions включают:

- `UIDocument.Selection.SetElementIds`;
- temporary Hide / Isolate;
- inverse isolation;
- Reset temporary visibility;
- создание / замена `ParameterFilterElement` и привязку к виду.

Semantic filter может быть корректным, но не полностью конвертируемым в native Revit filter. Native compatibility — отдельная host constraint.
