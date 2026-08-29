# Requirements

Этот раздел хранит нормализованный requirement set ContextFilter.

Исходные требования были переданы двумя customer documents и точечно уточнялись в общении. Они рассматриваются как **одна развивающаяся задача**.

## Документы

- [`functional-requirements.md`](functional-requirements.md) — функциональное поведение продукта;
- [`non-functional-requirements.md`](non-functional-requirements.md) — ограничения качества и требования устойчивости;
- [`business-rules.md`](business-rules.md) — правила, которые должны сохранять пользовательский смысл системы;
- [`acceptance-criteria.md`](acceptance-criteria.md) — исходная пустая формальная секция, derived acceptance baseline и фактическая приёмка.

## Эволюция

```text
Initial request
→ Category / Family / Type
→ Select / Hide / Isolate

Extended request
→ Current Selection context
→ parameter filtering
→ AND / OR
→ dynamic highlight
→ presets / templates
→ inverse / exclude actions

User testing
→ settings validation
→ lifecycle safety
→ failure visibility
→ host interaction fixes
→ performance stabilization
```

## Важное различие

```text
customer requirement
!= architecture decision
!= implementation optimization
```

Например, presets являются продуктовым требованием, а JSON persistence — уже технической реализацией. Быстрая фильтрация является ожидаемым качеством, а inverted index / parallel scan — способом реализации.

Traceability: [`../traceability/traceability-matrix.md`](../traceability/traceability-matrix.md).
