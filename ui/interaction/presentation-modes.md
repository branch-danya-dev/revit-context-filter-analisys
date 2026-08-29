# Presentation modes

ContextFilter supports two UI presentation modes over the same underlying workflow.

## DockablePane

`DockablePane` is the default mode in the delivered implementation and is treated as the more stable Revit 2025 presentation path.

It is suitable for a persistent working session where the user repeatedly changes context, parameters and actions without reopening the tool.

## FloatingWindow

`FloatingWindow` is an alternative presentation implemented through `MainFilterWindow` / `FilterWindowService`.

It changes window behavior and placement, not the semantics of filtering or actions.

## Invariant

```text
presentation mode
!= system mode
```

Changing DockablePane ↔ FloatingWindow must not change:

- `CollectionScope` meaning;
- `FilterDefinition`;
- evaluation result;
- preset meaning;
- Revit action semantics.

## Lifecycle note

Because ContextFilter runs inside Revit/WPF, UI attach/detach behavior is part of stability. Source analysis records a dockable-pane crash scenario and the correction that full UI attach is performed from an `ExternalCommand`, not opportunistically from `Idling`.

The Revit-side attach mechanics belong to `revit/`; this document only records the presentation invariant that a view must not trigger unsafe host work merely by existing or binding.
