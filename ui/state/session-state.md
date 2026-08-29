# Session state

ContextFilter distinguishes the existence of UI controls from an **active user session**.

## Active session

A session is active when the user is actually working with the filter UI and runtime work may be useful for the current interaction.

This distinction matters because source analysis records a final contract:

```text
IsUserSessionActive == false
→ no collect/index/highlight work on Idling
→ minimal background CPU
```

The actual `Idling` gating belongs to `revit/`, but UI owns the interaction state that determines whether the user session is active.

## Optional live behavior

The UI exposes convenience features such as:

- dynamic highlight;
- auto-refresh;
- hotkeys.

They are not required for a valid filter session and are default-off under calm settings.

```text
filter correctness
must not depend on
live convenience enabled
```

## Busy and progress state

Long-running collection/index work may make the UI busy and update progress. This state must correspond to the currently relevant request/session.

A stale response from an obsolete document/session must not revive old UI state.

## Closing the UI

Closing/hiding the active panel ends the need for heavy session-bound background work.

This does not mean:

```text
close UI
→ close Revit document
```

It means only that runtime work whose sole purpose is servicing the interactive panel should stop or remain dormant until the user resumes the session.
