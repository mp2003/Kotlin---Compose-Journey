# StoreProfileRoutes.kt — route definitions

**Code:** `.../presentation/navigation/StoreProfileRoutes.kt`

## Practice
- An `object StoreProfileRoutes` with `data object Graph : SimpleRoute("store_profile_graph")`
  and `data object StoreProfile : SimpleRoute("store_profile")`.

## Why
- Route strings live in one typed place, not scattered as string literals. `Graph` is the
  feature's entry point; `StoreProfile` is the screen inside it.
- `SimpleRoute` (from `:android-common`) is the base for argument-less routes. (Routes *with*
  args extend `ArgumentRoute` and build typed `navArgument` lists — not needed here.)

## Rule (ARCHITECTURE.md)
- Routes/Destinations/NavGraph live in `presentation/navigation/`.

## Graph vs screen route (learner note)
- A nested graph has its **own** route distinct from the screens inside it. Outsiders navigate to
  `Graph`; the graph's `startDestination` then shows `StoreProfile`. This is why there are two.

## See also
- [[presentation/navigation/nav-navgraph]] (uses both), [[presentation/navigation/nav-destinations]] (maps the screen route).
