# ContextFilter — системный анализ

> **SSAD-анализ реального реализованного плагина Autodesk Revit 2025 для контекстной фильтрации элементов BIM-модели.**

ContextFilter сначала ограничивает рабочую область явным контекстом, затем позволяет описать фильтр над этим контекстом, вычисляет результат вне прямых вызовов Revit API и только после этого применяет найденный набор обратно к Revit через выбранное действие.

<p>
  <a href="README.md">EN</a> · <a href="README_RU.md"><strong>RU</strong></a>
</p>

Репозиторий организован по **[SSAD — System-Structured Analysis Documentation](https://github.com/branch-danya-dev/ssad-methodology)** вокруг ответственности реальной системы, а не вокруг слоёв исходного кода `Domain / Application / Infrastructure / UI`.

---

## Контекст проекта

Это **завершённый реализованный проект**, публично реконструированный на основе двух видов evidence:

- исходных ТЗ заказчика с первоначальной потребностью и последующим уточнением требований;
- анализа исходного кода, фиксирующего реализованную архитектуру и поведение.

Изначальная проблема была прикладной: для поиска похожих элементов пользователю приходилось находить подходящий экземпляр на виде, выбирать его и пользоваться штатными командами Revit. Требовался быстрый способ выбирать, скрывать и изолировать элементы по категориям, семействам, типам и параметрам.

В ходе уточнения появились работа с предварительной выборкой, динамическое выделение, пресеты и шаблоны, логика И/ИЛИ и инверсионные действия. Реализованная система дополнительно содержит явные scope, историю фильтров, создание штатного фильтра Revit и runtime-механику для больших выборок.

> Исходные внутренние документы заказчика в публичный репозиторий не переносятся. Здесь хранится системное знание, которое они подтверждают, с разделением требований и implementation evidence.

---

## Система одним потоком

```text
REVIT DOCUMENT / VIEW / SELECTION
              ↓
           CONTEXT
              ↓
           CATALOG
              ↓
        FILTER INTENT
              ↓
        RESULT ELEMENTS
              ↓
            ACTION
              ↓
            REVIT
```

`Presets / History` возвращают сохранённый intent в фильтр, а `Runtime` отвечает за freshness, cache и responsiveness.

---

## Основные инварианты

1. **Источник истины по модели — Revit.** Snapshot и результат фильтра — производные знания.
2. **Контекст всегда явный:** active view, entire document и current selection — разные области.
3. **Смысл фильтра не равен механике Revit API.** Валидный фильтр не обязан быть представим штатным `ParameterFilterElement`.
4. **Фильтрация не меняет содержимое элементов модели.** Действия работают с selection/visibility/view configuration.
5. **Имя параметра не является его identity.** Важны источник и вид параметра: instance/type, built-in/shared/project/synthetic.
6. **Отсутствующий параметр не равен пустому значению.**
7. **Кэш не является authority.** Производный контекст допустим только пока он свеж.
8. **Доступ к Revit API проходит через контролируемую host boundary.**

---

## Структура ответственности

```text
system/          → сквозные инварианты и synthesis
interaction/     → запуск и пользовательская filter session
context/         → scope, candidate set и freshness
catalog/         → Category → Family → Type + параметры и значения
filtering/       → смысл условий и вычисление результата
actions/         → selection / visibility / native filter
presets/         → сохраняемый пользовательский intent и history
runtime/         → cache, invalidation, chunking, coalescing
revit-boundary/  → Revit authority и безопасный API access
evidence/        → требования, evolution и implementation traceability
```

Это **не template SSAD** и не копия solution-структуры. Такая форма следует конкретной системе ContextFilter.

---

## Ключевые различия

```text
Revit element
!= ElementSnapshot

Parameter display name
!= parameter identity

missing parameter
!= empty value

semantic filter
!= native Revit filter

filter result
!= action on result

cache hit
!= source authority
```

---

## Технический контекст

`Autodesk Revit 2025` · `C# / .NET 8` · `WPF` · `Revit API` · `ExternalEvent` · локальная JSON persistence

Реализация использует in-memory snapshots, несколько стратегий вычисления фильтра, cache invalidation, порционный сбор больших контекстов, persistent presets/history и конвертацию в штатный фильтр Revit там, где семантика совместима с ограничениями Revit API.

---

## Evidence

```text
Требования заказчика
        ↓
реализованное поведение
        ↓
implementation evidence
        ↓
SSAD synthesis
        ↓
актуальная системная модель
```

Начать историю требований: [`evidence/requirements-evolution.md`](evidence/requirements-evolution.md).

---

## Методология

**[SSAD — System-Structured Analysis Documentation](https://github.com/branch-danya-dev/ssad-methodology)**

> **Структуру знания определяет система, а не типы документов и не слои кода.**
