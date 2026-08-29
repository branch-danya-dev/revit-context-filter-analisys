# Logical Composition

Filter language построен как дерево `IFilterNode`.

Есть два вида узлов:

```text
FilterGroup
FilterCondition
```

## FilterGroup

Подтверждённые свойства:

```text
Id
LogicalOperator: And | Or
Negate
IsEnabled
Children
```

Группа может содержать другие группы и conditions, поэтому filter language поддерживает вложенную композицию.

## FilterCondition

Подтверждённые свойства:

```text
ParameterKey
Operator
Operands
IgnoreCase
```

Condition является атомарным утверждением над одним фильтруемым property identity.

## AND

Группа `And` требует совместного удовлетворения дочерних semantic conditions согласно evaluator contract.

## OR

Группа `Or` выражает альтернативные ветви filter intent.

## Negate

`Negate` меняет смысл узла как логического выражения, а не превращает каждый дочерний operator в отдельный обратный operator.

Это важно для сохранения структуры nested filters.

## IsEnabled

Domain явно хранит enabled state узла.

Однако предоставленный implementation analysis не фиксирует полную truth table для edge cases вроде пустой группы или группы, где все дети disabled. Поэтому этот документ **не придумывает** такое поведение.

Эти edge semantics должны подтверждаться кодом/тестами прежде, чем становиться более конкретным каноническим знанием.

## Инварианты

```text
logical tree
!= flat condition list
```

Преобразование/оптимизация допустимы только если сохраняют эквивалентную логическую семантику.
