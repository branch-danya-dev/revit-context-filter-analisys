# Жизненный цикл пресета в Application

Application управляет операциями вокруг `PresetDefinition`, не владея его смыслом Domain и физическим JSON-хранилищем.

```text
текущее состояние фильтра
→ BuildPresetUseCase
→ PresetDefinition
→ IPresetStore
→ адаптер Infrastructure
```

Подтверждены операции построения, сохранения, списка, удаления и обеспечения встроенных пресетов. В реализации есть пять встроенных шаблонов для стен, дверей, окон, колонн и балок.

- `PresetDefinition`, `Full`, `Template` → Domain;
- координация операций → Application;
- JSON, миграция схемы, атомарная запись → Infrastructure;
- список и команды пользователя → UI.

```text
условия пресета
!= формат хранения
!= текущий FilterResult
```
