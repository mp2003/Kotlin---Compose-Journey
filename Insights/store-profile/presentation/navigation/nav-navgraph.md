# StoreProfileNavGraph.kt — the feature graph

**Code:** `.../presentation/navigation/StoreProfileNavGraph.kt`

## Practice
- A public extension: `fun NavGraphBuilder.storeProfileNavGraph(navController)`.
- Internally a `FeatureNavigationGraph` subclass with `featureRoute = Graph.route`,
  `startDestination = StoreProfile.route`, registering `addDestinations(StoreProfileDestination(...))`
  inside `nestedGraph(...)`.

## Why
- The `NavGraphBuilder.xxxNavGraph(...)` extension is the feature's **single public entry point**
  for navigation. The landing host calls just this one function; it knows nothing of the screens inside.
- `nestedGraph` scopes the feature as a self-contained unit (your Week-4 nested-graph lesson,
  productionised).

## Rule (ARCHITECTURE.md)
- Feature exposes its nav graph via this extension; the host composes feature graphs without
  depending on their internals.

## How the host uses it (the last wiring step — not done yet)
In `LandingScreen.kt`'s `NavigationHost { … }`:
```
storeProfileNavGraph(navController.navController)
```
plus a drawer `NavigationDrawerItem` whose `onClick` does `onNavigateTo(StoreProfileRoutes.Graph.route)`.

## See also
- [[presentation/navigation/nav-routes]], [[presentation/navigation/nav-destinations]], [[README]] (full flow).
