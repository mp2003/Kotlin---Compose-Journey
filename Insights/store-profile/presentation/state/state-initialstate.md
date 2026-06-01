# StoreProfileInitialState.kt — the one-time screen-load state

**Code:** `.../presentation/state/StoreProfileInitialState.kt`

## Practice
- A `sealed class`: `Loading`, `Success`, `Error(title, message)`.
- Drives the *first* render: spinner → content, or → error screen.

## Why
- The initial fetch has three distinct outcomes that need three distinct full-screen UIs. A
  sealed class makes the `when` exhaustive and the RootScreen's `Crossfade` clean.

## Rule (ARCHITECTURE.md — the most-confused rule)
- **`InitialState` ≠ `isLoading`.**
  - `InitialState` = the one-time screen-load fetch (this file).
  - `isLoading` = a flag on `UiState` for *user-triggered* actions (pull-to-refresh, save…).
- Keep them separate. This view-only feature has no user actions, so it has `InitialState` and
  **no** `isLoading` — a clean example of the distinction.

## Gotcha
- Don't model the first load as `uiState.isLoading = true`. That conflates "screen is booting"
  with "user tapped something and we're busy." They render differently and recover differently.

## See also
- [[presentation/viewmodel/viewmodel]] (sets it), [[presentation/screen/screen-root]] (switches on it).
