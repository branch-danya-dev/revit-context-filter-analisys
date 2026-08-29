# Revit Host Boundary

This area owns **how ContextFilter crosses from plugin-side intent and derived knowledge into Autodesk Revit safely**.

## Canonical question

> What is Revit authoritative for, and how may an in-process WPF add-in safely read or change Revit-owned state?

## Revit authority

Revit is authoritative for:

- active document and view;
- element existence and native identities;
- current selection and view state;
- native parameter data;
- whether a native visibility/selection/filter action is accepted;
- Revit API thread and transaction constraints.

Physical co-location inside the Revit process does not eliminate this ownership boundary.

## Read boundary

The plugin reads source evidence through Revit API and converts it into derived ContextFilter structures:

```text
Revit elements
→ element IDs / classification / parameter evidence
→ snapshots / context / catalog
```

Snapshots are not Revit authority after source evidence changes.

## UI → Revit bridge

The implementation uses `ExternalEvent` as the controlled bridge from asynchronous WPF interaction to the Revit API main-thread model:

```text
UI intent
→ gateway / request queue
→ ExternalEvent
→ Revit-side request dispatcher
→ native API action
→ response
```

This implementation mechanism realizes the broader boundary rule that UI code must not treat Revit API access as unconstrained or thread-safe.

## Native realization

The boundary realizes supported actions such as:

- selection changes;
- temporary hide/isolate/reset;
- native `ParameterFilterElement` creation where compatible.

A Revit-native failure or incompatibility must remain visible to the caller. It must not silently redefine semantic filter meaning.

## Failure principle

```text
semantic result exists
+
Revit cannot realize requested action
→ action failure / incompatibility
→ semantic match set remains conceptually valid
```

## Does not own

- which elements should match → [`../filtering/`](../filtering/);
- which action the user intended → [`../actions/`](../actions/);
- cache freshness policy → [`../runtime/`](../runtime/).
