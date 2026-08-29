# Infrastructure

`ContextFilter.Infrastructure` поддерживает технические механизмы, которые не определяют Domain semantics.

Основные области:

- [`persistence/`](persistence/) — JSON stores и crash-safe writes;
- [`configuration/`](configuration/) — settings, schema migration и sanitization;
- [`logging/`](logging/) — application / performance logging;
- DI registration для инфраструктурных реализаций.
