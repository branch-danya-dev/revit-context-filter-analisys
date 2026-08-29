# System Evolution

После пользовательского тестирования система была стабилизирована по наблюдаемому поведению, а не только по исходному ТЗ.

## Изменения

- некорректные persisted settings → validation + normalization;
- ошибка загрузки presets → явное предупреждение пользователю;
- background work после закрытия документа → остановка document-bound operations;
- UI state при смене документа → automatic reset;
- hotkeys always-on → opt-in;
- auto-open при старте Revit → запуск только из Ribbon;
- `Ctrl + Click` конфликтовал с native multi-selection → `Ctrl + Shift + Click`;
- event handlers создавали лишнюю нагрузку → активируются только во время использования;
- built-in presets создавались при старте → lazy initialization при первом открытии панели;
- isolation выполнялась вне требуемого transaction context → исправлено;
- shutdown задерживался → оптимизировано освобождение ресурсов.

## Вывод

```text
implementation feedback
→ reopen affected system knowledge
→ correct behavior
→ verify again
```
