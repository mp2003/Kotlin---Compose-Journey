# StoreProfileUiState.kt — the single state object

**Code:** `.../presentation/state/StoreProfileUiState.kt`

## Practice
- One immutable `@Stable data class` holding everything the screen renders. Here: `profile: StoreProfile?`.

## Why
- MVI's "single source of truth" for the UI. The screen is a pure function of this object —
  same state ⇒ same pixels.
- Immutable + updated via `.copy()` ⇒ Compose reliably detects change and recomposes.

## Rule (ARCHITECTURE.md / app practice #2)
- `UiState` is an immutable data class. UI reads it; never mutates it.

## Gotcha
- `@Stable` is a hint to Compose that this type's equality is well-behaved — helps skip
  unnecessary recompositions. Standard on UiState classes here.
- For a view-only screen this is tiny. A screen with user actions would add flags like
  `isLoading`, `isRefreshing`, selected items, etc. (but **not** the one-time load state — that's
  [[presentation/state/state-initialstate]]).

## See also
- [[presentation/viewmodel/viewmodel]] (owns + copies it), [[presentation/screen/screen-pure]] (renders it).
