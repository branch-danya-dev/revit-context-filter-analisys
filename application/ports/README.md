# Application Ports

Ключевой host port — `IRevitGateway`.

Через него Application запрашивает:

- context info / context collection;
- parameter index / values;
- selection actions;
- visibility actions;
- native filter creation.

Другие порты включают preset store, filter history, settings/preferences, logging и UI-support services.

Порт определяет требуемую способность системы; конкретная реализация находится вне Application.
