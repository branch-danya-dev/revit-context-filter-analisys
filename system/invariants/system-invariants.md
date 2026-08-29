# System Invariants

## SI-01 · Revit remains host authority

ContextFilter не становится владельцем Revit document, element, view или selection state.

## SI-02 · Filter meaning is independent from Revit realization

`FilterDefinition` определяет смысл фильтра. Возможность создать `ParameterFilterElement` — отдельная compatibility concern.

## SI-03 · Parameter identity is not a display label

Фильтр должен опираться на стабильный `ParameterKey`, а не только на отображаемое имя параметра.

## SI-04 · Missing and empty are different states

Отсутствующий параметр и существующий параметр без значения не должны автоматически трактоваться одинаково.

## SI-05 · Not loaded is not missing

Lazy snapshot может ещё не содержать parameter data. Это не доказывает отсутствие параметра у Revit element.

## SI-06 · Filter result and action are different concepts

Один matched set может использоваться для selection, hide, isolate или native-filter action без изменения смысла самого фильтра.

## SI-07 · Evaluation strategy must preserve semantics

Inverted index, sequential scan и parallel scan должны приводить к одному логическому результату при одинаковых входных данных.

## SI-08 · Derived state has freshness boundaries

Cache, tree, indexes и FilterResult действительны только относительно Revit context, из которого они были получены.

## SI-09 · Document lifecycle invalidates document-bound work

После закрытия / смены документа старые background tasks и document-specific UI state не должны продолжать жить как будто context всё ещё действителен.

## SI-10 · Inactive plugin must not impose heavy background load

При закрытой панели / неактивной пользовательской сессии тяжёлые Idling collect/index/highlight операции не выполняются.

## SI-11 · Plugin interaction must respect native Revit interaction

Hotkeys и mouse gestures не должны без необходимости перехватывать established host semantics. Реальный конфликт `Ctrl + Click` с multi-selection привёл к изменению комбинации и opt-in политике hotkeys.

## SI-12 · Persisted configuration is validated before use

Некорректные settings не должны напрямую становиться runtime configuration.

## SI-13 · Failure must not masquerade as a valid empty state

Ошибка collection, preset loading или action execution должна быть отличима от корректного результата «ничего не найдено / нечего применить».

## SI-14 · Revit write actions obey host transaction rules

Действия, для которых Revit требует transaction context, должны выполняться внутри корректной Revit transaction boundary.

## SI-15 · Source BIM elements are not filter output

Filter workflow не должен удалять, перемещать или произвольно изменять исходные элементы модели.
