# User Interaction

This area owns **how the user establishes filtering intent and triggers system behavior**.

## Canonical question

> How does a user enter a ContextFilter session, narrow the current context, build conditions and choose an action?

## Main interaction

The refined requirement described a three-zone dialog. The implemented UI preserves the same semantic progression even where the visual representation evolved:

```text
1. Context / categories / types
        ↓
2. Parameters
        ↓
3. Values / active filter / result
        ↓
Action
```

The user may start from:

- the active view with no preselection;
- an existing Revit selection;
- the plugin UI/ribbon;
- optional hotkey-driven entry where enabled.

## Intent vs side effect

Changing categories, parameters or values changes **filter intent**. It does not itself modify model elements.

Dynamic highlighting is an optional preview behavior and must remain distinguishable from an explicit final action.

## Action availability

Some actions depend on session context. For example, exclusion from an existing selection only makes sense when there is a selection set to subtract from.

The UI may disable impossible actions, but the underlying validity rule belongs to the action/context owners rather than to button state.

## Does not own

- which elements belong to scope → [`../context/`](../context/);
- parameter semantics → [`../catalog/`](../catalog/);
- logical condition semantics → [`../filtering/`](../filtering/);
- Revit API execution → [`../revit-boundary/`](../revit-boundary/).
