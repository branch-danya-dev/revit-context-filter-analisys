# Revit Collection

`RevitElementCollector` получает элементы по выбранному Domain `CollectionScope`.

Host layer также читает Category / Family / Type records и строит light element snapshots.

Для больших выборок реализована chunked collection через Revit Idling, чтобы длительная операция не блокировала host UI одним большим вызовом.

Performance thresholds являются деталями реализации, а не частью Domain meaning.
