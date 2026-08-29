# Application

`ContextFilter.Application` координирует пользовательские сценарии поверх Domain и внешних портов.

## Основные ответственности

- [`use-cases/`](use-cases/) — прикладные сценарии;
- [`context/`](context/) — построение дерева и parameter index;
- [`filtering/`](filtering/) — evaluation strategies;
- [`actions/`](actions/) — расчёт selection / visibility sets;
- [`ports/`](ports/) — контракты к Revit, persistence и UI-support services.

Application не должен напрямую обращаться к Revit API.
