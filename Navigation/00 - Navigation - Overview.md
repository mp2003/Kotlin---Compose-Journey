###### Elevator Pitch
- Jetpack Navigation Compose lets you move between screens using a single `NavHost`, a `NavController`, and string routes — keeping every screen a dumb, reusable composable that knows nothing about navigation.

---

###### Definition
- A Compose library that models app navigation as a graph of routes, where one `NavHost` swaps screens and a `NavController` drives the back stack

---

###### Real-World Analogy
- `NavController` -> a GPS that knows where you are and where you can go
- `NavHost` -> the road map listing every destination
- Route string -> a street address
- Back stack -> breadcrumbs you dropped to find your way home

---

###### What
- `NavController` -> the object that performs navigation and owns the back stack
- `NavHost` -> the composable that hosts the graph and renders the current screen
- `composable(route)` -> registers one screen against a route string
- `NavGraphBuilder` -> the DSL scope inside `NavHost { }` where you declare destinations
- Arguments -> values passed through the route string (`/{name}/{age}`)
- Nested graph -> a sub-graph (`navigation(route = ...)`) that bundles related screens

---

###### Why
- One source of truth for what screens exist and how they connect
- Screens stay decoupled — they take lambdas, never the `NavController`
- Back stack handled for you — no manual screen stack management
- Nested graphs scale: each feature flow (auth, onboarding) is a movable unit

---

###### Sub-Pages
- [[01 - NavHost and NavController]] -> the core setup: graph + controller
- [[02 - Arguments Passing]] -> sending typed data through routes
- [[03 - Back Navigation]] -> popBackStack and the hoisted lambda pattern
- [[04 - Numeric TextField Input]] -> keyboard type vs. real validation
- [[05 - Nested Graphs]] -> bundling screens into scoped sub-graphs

---

###### How it Works
- App sets `NavHost(navController, startDestination)` as the UI root
- `NavHost` reads its `startDestination` route and renders that screen
- A button calls a hoisted lambda -> the graph calls `navController.navigate(route)`
- `NavController` pushes the new screen onto the back stack
- System Back (or `popBackStack()`) pops the top screen off the stack
- `NavHost` re-renders whatever route is now on top

---

###### Syntax
- `rememberNavController()` -> creates and remembers the controller across recompositions
- `NavHost(navController, startDestination)` -> hosts the graph, picks first screen
- `composable(route) { }` -> registers a destination
- `navController.navigate(route)` -> go to a screen
- `navController.popBackStack()` -> go back one screen

```kotlin
// Minimal skeleton
@Composable
fun AppNav() {
    val navController = rememberNavController()              // controller
    NavHost(
        navController = navController,
        startDestination = "main_screen"                    // first screen
    ) {
        composable("main_screen") { /* MainScreen() */ }    // a destination
        composable("detail_screen") { /* DetailScreen() */ }
    }
}
```

---

###### Layered Diagram

```text
+-----------------------------+
|  Screens (Composables)      |  dumb UI, take lambdas only
+-----------------------------+
|  NavGraphBuilder DSL        |  composable(...) / navigation(...)
+-----------------------------+
|  NavHost                    |  renders current route
+-----------------------------+
|  NavController + BackStack   |  drives navigation, owns history
+-----------------------------+
```

---

###### Memory Hook
- Controller drives, Host shows, Routes are addresses, Lambdas keep screens dumb

---

###### Key Rule
- Screens never receive `NavController` — they expose lambdas; the graph owns all wiring

---

###### Related
- [[01 - NavHost and NavController]]
- [[05 - Nested Graphs]]
