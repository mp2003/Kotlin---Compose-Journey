# StoreProfileNavigation.kt — navigatable commands (RootScreen → Destination)

**Code:** `.../presentation/state/StoreProfileNavigation.kt`

## Practice
- A separate `sealed class` of *only* navigation commands: `NavigateBack`.
- The RootScreen translates an Effect into one of these and passes it up via `onNavigationChange`.

## Why
- It separates "I want to navigate" (a pure command) from "how the NavController does it"
  (lives in the Destination). The screen stays decoupled from the nav graph — preview/test friendly.
- This is the second half of the **Effect vs Navigation** split.

## Rule (ARCHITECTURE.md / practice #7)
- `Navigation` (RootScreen→Destination) carries ONLY navigatable commands. RootScreen handles
  non-nav effects locally (toast) and forwards nav ones via `onNavigationChange`.

## Why TWO sealed classes for what looks like the same thing?
- `Effect.NavigateBack` is the ViewModel saying "a nav event occurred."
- `Navigation.NavigateBack` is the RootScreen saying "Destination, please pop."
- The Destination owns the `NavController`; the RootScreen and ViewModel never touch it.
  This indirection is what keeps the Screen pure and the ViewModel framework-light.

## See also
- [[presentation/state/state-effect]], [[presentation/navigation/nav-destinations]] (consumes these).
