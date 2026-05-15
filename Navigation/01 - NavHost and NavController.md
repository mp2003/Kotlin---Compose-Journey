---
up: "[[00 - Navigation - Overview]]"
---

###### Elevator Pitch
- `NavHost` is the container that renders whichever screen matches the current route, and `NavController` is the object you call to move between those routes and manage the back stack.

---

###### Definition
- `NavController` -> the navigation engine and back-stack owner; `NavHost` -> the composable that hosts the route graph and shows the active destination

---

###### Real-World Analogy
- `NavController` -> a remote control
- `NavHost` -> the TV screen showing one channel at a time
- Route -> the channel number you press
- `startDestination` -> the channel the TV turns on to

---

###### What
- `rememberNavController()` -> builds a controller that survives recomposition
- `NavHost` -> takes the controller + a `startDestination` route
- `composable(route) { }` -> declares one screen for one route
- `startDestination` -> the route shown when the host first appears
- Routes are plain `String`s, kept in a sealed class for safety

---

###### Why
- Centralizes "what screens exist" in one readable graph
- `NavController` removes manual back-stack bookkeeping
- A sealed `ScreenState` avoids typo'd route strings scattered everywhere

---

###### Core Concepts
- `NavController` -> performs `navigate()` / `popBackStack()`, owns history
- `NavHost` -> watches the controller, renders the matching `composable`
- `NavGraphBuilder` -> the `{ }` scope where `composable(...)` calls live
- Sealed route class -> one place that lists every route string
- `startDestination` -> entry point of the graph
- Link: [[00 - Navigation - Overview]]

---

###### How it Works
- `rememberNavController()` creates the controller
- `NavHost` is placed as the screen root with that controller
- `NavHost` reads `startDestination` and renders that `composable`
- User action -> a hoisted lambda -> `navController.navigate("detail_screen")`
- `NavHost` recomposes and shows the `detail_screen` composable
- The previous screen is kept on the back stack

---

###### Syntax
- `val navController = rememberNavController()` -> create controller
- `NavHost(navController = ..., startDestination = ScreenState.MainScreen.route)` -> host graph
- `composable(route = ScreenState.MainScreen.route) { }` -> register a screen
- `sealed class ScreenState(val route: String)` -> typed route catalog
- `object MainScreen : ScreenState("main_screen")` -> one route entry

```kotlin
// Route catalog — one place for all route strings
sealed class ScreenState(val route: String) {
    object MainScreen : ScreenState("main_screen")
    object DetailScreen : ScreenState("detail_screen")
}

@Composable
fun NavigationMvi() {
    val navController = rememberNavController()                 // controller
    NavHost(
        navController = navController,
        startDestination = ScreenState.MainScreen.route         // first screen
    ) {
        composable(ScreenState.MainScreen.route) {
            MainScreenMvi(navController = navController)         // see note 03 for cleaner version
        }
        composable(ScreenState.DetailScreen.route) {
            // DetailScreen(...)
        }
    }
}
```

---

###### ASCII Flowchart

```text
[ rememberNavController() ]
        |
        v
[ NavHost(startDestination = "main_screen") ]
        |
        v   reads startDestination
[ composable("main_screen") renders MainScreen ]
        |
        v   user taps button -> navigate("detail_screen")
[ NavController pushes detail_screen onto back stack ]
        |
        v
[ NavHost renders DetailScreen ]
```

---

###### Common Mistakes
- BAD: creating `NavController` with `NavController(context)` instead of `rememberNavController()` -> not recomposition-safe
- BAD: hardcoding raw `"main_screen"` strings everywhere -> typos break navigation silently
- BAD: putting `NavHost` inside a scrolling/conditional block -> recreated graph, lost back stack
- BAD: passing `NavController` deep into child composables -> tight coupling (see note 03)

---

###### Common Follow-up Traps
- Q: Why `rememberNavController()` and not just `NavController()`?
  A: `remember` keeps the same controller across recompositions; a fresh one each recompose would reset navigation
- Q: Where does the back stack live?
  A: Inside the `NavController` — `NavHost` just renders whatever route is on top
- Q: Why a sealed class for routes?
  A: Compile-time safety + one source of truth; you reference `ScreenState.X.route`, not loose strings

---

###### Memory Hook
- Remember the controller, host the graph, start at one route

---

###### Key Rule
- One `NavHost`, created once at the screen root, with a remembered `NavController`

---

###### Related
- [[00 - Navigation - Overview]]
- [[02 - Arguments Passing]]
