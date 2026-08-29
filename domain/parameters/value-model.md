# Parameter Value Model

Значение параметра хранится как domain `ParameterValue`, связанный с `ParameterKey`.

Implementation analysis подтверждает typed value model через `ParameterValueKind`, включающий как минимум:

```text
String
Integer
Double
Boolean
ElementId
...
```

## Почему тип важен

Фильтрация не должна сводить все значения к строковому сравнению.

```text
"10"
!=
10 as integer
!=
10.0 as numeric quantity
!=
ElementId(10)
```

Конкретное преобразование Revit storage type в `ParameterValue` принадлежит `revit/parameters/`, но полученный Domain value должен сохранять достаточный semantic type.

## Presence semantics

В filter language существуют отдельные операторы:

```text
Exists / NotExists
IsEmpty / IsNotEmpty
```

Следовательно, Domain различает как минимум два разных вопроса:

```text
Есть ли параметр?
!=
Есть ли у существующего параметра содержательное значение?
```

Quick-filter implementation также отдельно компилирует специальные состояния `__missing__` и `__empty__` в `NotExists` и `IsEmpty`.

## Partial snapshot caveat

```text
NOT LOADED
!= MISSING
```

Технически ещё не загруженное значение не может автоматически трактоваться как `NotExists`.

## Инварианты

1. Comparison semantics должны учитывать kind значения.
2. Missing и empty — разные состояния.
3. Display-formatted value не обязано быть canonical comparison value.
4. Нормализация не должна менять предметный смысл значения.
5. Техническая неполнота snapshot не должна создавать ложное missing state.
