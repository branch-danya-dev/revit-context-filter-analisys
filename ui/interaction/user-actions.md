# User actions

UI exposes actions over the current evaluated result, but does not perform Revit-side effects itself.

## Selection

```text
user clicks action
→ UI command
→ Application target-set calculation
→ IRevitGateway
→ Revit adapter
```

Supported selection intents:

- `Replace`;
- `Add`;
- `Exclude`.

The UI may display the action as a button, but the action semantics are not owned by WPF.

## Visibility

Supported intents:

- temporary hide;
- temporary isolate;
- inverse isolate;
- reset temporary visibility.

Again, the UI owns command initiation and visible busy/error state. Revit owns actual view mutation.

## Native filter

Creating a native Revit filter is not equivalent to evaluating ContextFilter's semantic filter.

```text
semantic FilterDefinition
→ compatibility check
→ native-filter action request
→ Revit realization
```

If the semantic filter is incompatible with Revit native filter capabilities, UI must surface that condition rather than silently changing the filter meaning.

## Dynamic highlight

Dynamic highlight is a convenience interaction mode, not a requirement for semantic correctness. It is opt-in/default-off in current calm settings and must remain bounded so that UI convenience does not impose continuous host load.

## Invariant

```text
button enabled
!= action guaranteed to succeed
```

A valid UI command can still fail downstream because of current document/view state, Revit restrictions or native-filter incompatibility. Such failures must return as explicit UI feedback.
