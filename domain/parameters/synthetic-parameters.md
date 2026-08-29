# Synthetic Parameters

ContextFilter расширяет фильтруемую модель свойствами, которые представлены как Domain parameters, хотя не обязательно существуют как обычные Revit `Parameter` objects.

Подтверждённые synthetic keys:

| Key | Смысл |
|---|---|
| `Category` | категория элемента |
| `Family` | семейство |
| `TypeName` | имя типа |
| `ElementId` | числовой id элемента |
| `UniqueId` | стабильный Revit UniqueId |
| `Workset` | рабочий набор |
| `Level` | уровень |

## Зачем это нужно

Пользователь формулирует фильтр в едином языке:

```text
FilterCondition
→ ParameterKey
→ operator
→ operands
```

Неважно, происходит ли значение из native parameter или из вычисляемого свойства элемента.

## Граница

```text
Synthetic domain parameter
!=
Native Revit Parameter
```

Synthetic property может участвовать в client-side filter, даже если для неё нет прямого native parameter representation.

Это также означает, что:

```text
client-side valid condition
!=
native-filter-compatible condition
```

## Ownership

Domain владеет semantic identity synthetic property.

Revit layer владеет способом получить/вычислить конкретное значение из host model.
