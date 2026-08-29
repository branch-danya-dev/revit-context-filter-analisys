# Application Context Processing

Application строит производные представления над собранными элементами:

```text
ElementTreeRecord[]
→ Category → Family → Type

ElementSnapshot[]
→ parameter index
→ unique parameter values
```

`ContextCollectionCache` хранит derived tiers, но cache hit не меняет source authority: актуальность определяется относительно Revit document/view/selection state.
