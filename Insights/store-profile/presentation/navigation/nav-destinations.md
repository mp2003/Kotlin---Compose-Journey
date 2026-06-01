# StoreProfileDestinations.kt — route → screen binding

**Code:** `.../presentation/navigation/StoreProfileDestinations.kt`

## Practice
- `class StoreProfileDestination(navController) : BaseDestination()`.
- `route` = `StoreProfileRoutes.StoreProfile`; `screen` = a composable that renders
  `StoreProfileRootScreen(onNavigationChange = { when(it) { NavigateBack → navController.popBackStack() } })`.

## Why
- This is the **only** place that touches the `NavController`. It turns the screen's abstract
  `Navigation` command into a concrete NavController call.
- `BaseDestination` gives default slide transitions for free.

## Rule (ARCHITECTURE.md / practice #7, #8)
- Keeps the NavController out of the RootScreen and Screen — the whole point of the
  `Navigation` sealed hierarchy.

## Gotcha
- The `when (navigation)` is exhaustive over `StoreProfileNavigation`. Add a nav command and the
  compiler makes you handle it here. That's the safety the sealed class buys.

## See also
- [[presentation/state/state-navigation]] (what it consumes), [[presentation/screen/screen-root]] (what it hosts), [[presentation/navigation/nav-navgraph]] (what registers it).
