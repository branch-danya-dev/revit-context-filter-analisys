# Sanitization

`JsonSettingsStore` применяет `Sanitize()` к загруженной конфигурации.

## Назначение

Sanitization защищает runtime от некорректных или чрезмерных persisted values, в том числе от значений, которые могли быть внесены вручную в `settings.json`.

```text
untrusted persisted settings
→ clamp / normalize / validate
→ bounded runtime settings
```

## Почему это Infrastructure responsibility

Application потребляет настройки через абстракцию. Оно не должно знать, был ли источник JSON-файлом и каким образом исправлялись некорректные persisted values.

## Системный вывод

```text
successful deserialization
!= valid configuration
```

Это правило появилось не как теоретическая перестраховка: во время пользовательского тестирования invalid settings уже приводили к ошибкам, после чего validation/normalization была добавлена в продукт.

## Verification

В implementation evidence присутствуют `AppSettingsSanitizeTests`, проверяющие clamping и migration v2.

## Граница evidence

Источник подтверждает наличие `Sanitize()` и clamping-тестов, но не перечисляет полный набор min/max для каждого setting. Эти численные границы здесь намеренно не реконструируются.
