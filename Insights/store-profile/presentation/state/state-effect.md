# StoreProfileEffect.kt — one-shot events (ViewModel → RootScreen)

**Code:** `.../presentation/state/StoreProfileEffect.kt`

## Practice
- A `sealed class` of one-time events: `ShowToast(message)`, `NavigateBack`.
- Emitted via a `SharedFlow`, collected once in the RootScreen's `LaunchedEffect`.

## Why
- State is *sticky* (re-read on every recompose); Effects are *consumed once*. A toast must fire
  once, not every recompose — so it's an Effect, not state.
- Effect carries **all** one-shot events, including navigation ones.

## Rule (ARCHITECTURE.md / practice #7)
- `Effect` (ViewModel→RootScreen) carries ALL one-time events. It is **distinct** from
  `Navigation`. Don't merge them.

## State vs Effect (the classic learner trap)
- Will the user need to "see it again" after rotation? → **State**.
- Is it a fire-once side effect (toast, haptic, navigate)? → **Effect**.

## See also
- [[presentation/state/state-navigation]] (nav-only subset), [[presentation/screen/screen-root]] (collects effects).
