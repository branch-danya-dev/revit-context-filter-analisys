# Domain Result

Фильтрация производит derived result — набор идентификаторов элементов, удовлетворяющих `FilterDefinition` в текущем контексте.

```text
fresh snapshots
+ FilterDefinition
→ FilterResult
```

`FilterResult`:

- не является новым источником истины;
- становится устаревшим при изменении filter intent или source context;
- не определяет дальнейшее действие пользователя.
