# Координация контекста

Этот раздел описывает, как Application превращает полученный от Revit набор кандидатов в производные структуры, необходимые интерфейсу и движку фильтрации.

- [`context-orchestration.md`](context-orchestration.md) — получение и подготовка контекста;
- [`projection-pipeline.md`](projection-pipeline.md) — дерево, индекс параметров и значения;
- [`cache-and-invalidation.md`](cache-and-invalidation.md) — зависимость уровней производного состояния.

Application не собирает `Element` через Revit API. Он запрашивает данные через порт и работает с представлениями Domain/Application.

```text
сбор в Revit
→ ответ порта
→ проекции / кэш Application
→ UI + фильтрация
```
