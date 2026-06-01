# StoreProfileScreen.kt — the pure UI

**Code:** `.../presentation/screen/StoreProfileScreen.kt`

## Practice
- Signature: `StoreProfileScreen(uiState, modifier, onBack)` — **state in, callbacks out**.
- No ViewModel, no Effect collection, no NavController. Just renders `uiState` and calls
  `onBack` when the back button is tapped.
- Has a `@Preview` using a hand-built `StoreProfileUiState` — possible *because* it's pure.

## Why
- A pure composable is a function of its inputs ⇒ trivially previewable and testable. This is
  the "state hoisting" idea from Compose basics, applied at screen scale.
- All the messy wiring lives one level up in [[presentation/screen/screen-root]].

## Rule (ARCHITECTURE.md / practice #8)
- `[Feature]Screen` is a pure, testable composable taking state + callbacks.

## Gotcha
- Uses Andromeda design-system types: `design.andromedacompose.components.Text` and
  `AndromedaTheme.typography.*`. Valid style names (from the andromeda jar):
  `titleHeroTextStyle`, `titleModerateDemiTextStyle`, `titleModerateBoldTextStyle`,
  `bodyModerateDefaultTypographyStyle`. **There is no `titleModerateTextStyle`** — I hit that
  compile error and fixed it; double-check style names against the jar, not autocomplete memory.

## See also
- [[presentation/state/state-uistate]] (its only input), [[presentation/screen/screen-root]] (its caller).
