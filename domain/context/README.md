# Domain Context Model

Канонические scope:

- `ActiveView`;
- `EntireDocument`;
- `CurrentSelection`.

`ElementSnapshot` — in-memory представление элемента для клиентской фильтрации. Оно сохраняет source identifiers, category/family/type identity и параметрические значения, но не становится authoritative Revit element.

Light snapshot может быть дополнен параметрами лениво.

```text
not loaded
!= parameter missing
```
