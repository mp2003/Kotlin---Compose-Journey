# StoreProfileAction.kt — user intents

**Code:** `.../presentation/state/StoreProfileAction.kt`

## Practice
- A `sealed class` of every thing the user/UI can do: `OnBack`, `OnScreenResumed`.
- The screen sends these to a single `onAction(action)` entry point.

## Why
- "Intent" in MVI = the I. One funnel for all input means one place to reason about behaviour.
- Sealed ⇒ the `when (action)` in the ViewModel is exhaustive; add a case and the compiler
  forces you to handle it.

## Rule (app practice #2)
- Unidirectional: `Action → ViewModel.onAction → UiState (.copy) → UI`. Actions go up only.

## Gotcha
- Actions describe **what happened** ("OnBack"), not **what to do** ("popBackStack"). The
  decision of what to do lives in the ViewModel. Keep verbs out of action names.

## See also
- [[presentation/viewmodel/viewmodel]] (handles them), [[presentation/state/state-effect]] (the ViewModel's reply).
