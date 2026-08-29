# Actions

This area owns **what the system may do with an evaluated element set**.

## Canonical question

> Given a current matched set, which explicit Revit-side action is requested and what does success mean?

## Action families

### Selection

```text
Replace
Add
Exclude
```

Selection actions change Revit selection state. They do not modify source element content.

### Temporary visibility

```text
HideTemporary
IsolateTemporary
IsolateInverse
ResetTemporary
```

Visibility actions operate on the current view state and must remain distinguishable from permanent source-model changes.

### Native Revit filter

The current semantic filter may be converted into a native `ParameterFilterElement` when its categories, parameters, logical structure and operators are representable by Revit.

```text
Semantic filter valid
        ↓
Native-compatible?
   no → semantic filtering still valid
  yes
   ↓
create / replace / rename native filter
```

Native-filter incompatibility is not a failure of ContextFilter's own evaluation semantics.

## Safety boundary

The customer requirements explicitly excluded deletion, movement or arbitrary modification of filtered elements.

Therefore:

> **A matched set grants permission to perform the selected supported action, not permission to mutate the matched elements generally.**

## Result vs action

```text
FilterResult
!= Selection
!= Visibility state
!= ParameterFilterElement
```

One result can support multiple actions without changing why those elements matched.

## Does not own

- how matches are computed → [`../filtering/`](../filtering/);
- Revit API mechanics / transactions → [`../revit-boundary/`](../revit-boundary/);
- button availability and presentation → [`../interaction/`](../interaction/).
