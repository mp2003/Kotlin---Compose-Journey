---
up: "[[00 - Navigation - Overview]]"
---

###### Elevator Pitch
- A nested graph bundles related screens under one parent route using `navigation(route = "auth", startDestination = "login") { }`, so you can enter the whole flow by its route and clear it in one shot with `popUpTo(authRoute) { inclusive = true }`.

---

###### Definition
- A sub-graph inside the `NavHost` that groups screens as a single scoped, named unit with its own route distinct from the screens it contains

---

###### Real-World Analogy
- Nested graph -> a department in a building (the "Auth" department)
- Parent route `"auth"` -> the department's front door
- `startDestination` -> the reception desk you always hit first
- `popUpTo("auth") { inclusive = true }` -> evacuating the entire department at once

---

###### What
- `navigation(startDestination, route) { }` -> declares the sub-graph
- The graph's `route` ("auth") is separate from child routes ("login", "register")
- `NavGraphBuilder.authGraph()` -> an extension function holding the sub-graph
- `popUpTo(graphRoute) { inclusive = true }` -> pop the whole flow off the stack

---

###### Why
- Scopes a feature flow (auth, onboarding, checkout) into one movable unit
- The graph's own route lets you navigate to / clear the whole flow at once
- Extension function keeps `NavHost` clean as the app grows
- After finishing a flow, clearing it stops Back from re-entering it

---

###### Core Concepts
- Nested `navigation(...)` -> a graph within a graph
- Graph route != screen routes -> you navigate to `"auth"`, it shows `"login"`
- `NavGraphBuilder` extension -> reusable, self-contained flow definition
- `popUpTo(route) { inclusive = true }` -> remove everything up to AND including that route
- Link: [[00 - Navigation - Overview]]

---

###### How it Works
- `NavHost` `startDestination` set to the graph route `"auth"`
- Entering `"auth"` transparently shows its `startDestination` `"login"`
- Login button -> `navigate("register")`
- Register "finish" -> `navigate("main_screen")` with `popUpTo("auth") { inclusive = true }`
- The entire auth sub-graph is removed from the back stack
- Back from MainScreen now exits the app instead of returning to auth

---

###### Syntax
- `fun NavGraphBuilder.authGraph(navController: NavController)` -> extension on the DSL scope
- `navigation(startDestination = Login.route, route = AuthGraph.route) { }` -> nested graph
- `composable(Login.route) { }` inside the nested block -> a child screen
- `navigate(Main.route) { popUpTo(AuthGraph.route) { inclusive = true } }` -> clear flow
- `authGraph(navController)` called inside `NavHost { }` -> plug the sub-graph in

```kotlin
// AuthGraph.kt — the nested flow as a reusable extension
fun NavGraphBuilder.authGraph(navController: NavController) {
    navigation(
        startDestination = ScreenState.Login.route,   // enters here
        route = ScreenState.AuthGraph.route           // graph's OWN route "auth"
    ) {
        composable(ScreenState.Login.route) {
            LoginScreen(
                onLoginClick = {
                    navController.navigate(ScreenState.Register.route)
                }
            )
        }
        composable(ScreenState.Register.route) {
            RegisterScreen(
                onRegisterClick = {
                    navController.navigate(ScreenState.MainScreen.route) {
                        popUpTo(ScreenState.AuthGraph.route) {
                            inclusive = true          // clear the WHOLE auth flow
                        }
                    }
                },
                onBackClick = { navController.popBackStack() }
            )
        }
    }
}

// NavigationMvi.kt — plug it into the host
NavHost(navController, startDestination = ScreenState.AuthGraph.route) {
    composable(ScreenState.MainScreen.route) { /* ... */ }
    composable(ScreenState.DetailScreen.route + "/{name}/{age}", /* args */) { /* ... */ }
    authGraph(navController = navController)            // nested graph added here
}
```

---

###### ASCII Flowchart

```text
[ App start: NavHost startDestination = "auth" ]
        |
        v   "auth" -> its startDestination
[ LoginScreen ]
        |
        v   onLoginClick -> navigate("register")
[ RegisterScreen ]
        |
        v   onRegisterClick -> navigate("main_screen")
        |                       popUpTo("auth") inclusive = true
[ Whole "auth" graph popped off back stack ]
        |
        v
[ MainScreen ]  -- press Back --> app exits (no return to auth)
```

---

###### Layered Diagram

```text
+-----------------------------------+
|  NavHost                          |
|  +-----------------------------+  |
|  |  navigation(route="auth")   |  |  <- nested graph
|  |   - Login  (start)          |  |
|  |   - Register                |  |
|  +-----------------------------+  |
|  - MainScreen                     |
|  - DetailScreen/{name}/{age}      |
+-----------------------------------+
```

---

###### Common Mistakes
- BAD: giving the graph the same route as a child screen -> ambiguous, broken navigation
- BAD: skipping `popUpTo` -> Back from Main walks back into Register/Login
- BAD: `popUpTo("auth")` without `inclusive = true` -> the auth graph itself stays on the stack
- BAD: screen files left in the default package -> bare `import LoginScreen` breaks on move/refactor
- BAD: passing `NavController` into Login/Register screens -> coupling; use hoisted lambdas

---

###### Weak Area Clarification
- Confusion: "Why does the graph need its OWN route separate from login/register?"
- Why: without a graph route there's nothing to target when clearing the whole flow
- Resolution: the graph route ("auth") is the single handle you `popUpTo` to wipe every screen inside it at once

---

###### Common Follow-up Traps
- Q: What does `inclusive = true` do in `popUpTo`?
  A: Pops everything up to that route AND the route itself — so the auth graph is fully gone
- Q: Why an extension on `NavGraphBuilder`?
  A: It can be called inside `NavHost { }` like `composable(...)`, keeping the host clean and the flow reusable
- Q: You navigate to `"auth"` but see Login — why?
  A: A graph has no UI of its own; it transparently shows its `startDestination`
- Q: When do real apps use nested graphs?
  A: Per feature flow — auth, onboarding, checkout — each a scoped, independently clearable unit

---

###### Memory Hook
- The graph is a department with its own door ("auth"); `popUpTo inclusive` evacuates the whole department

---

###### Key Rule
- A nested graph has its own route distinct from its screens — that route is what `popUpTo(... ) { inclusive = true }` clears in one move

---

###### Related
- [[03 - Back Navigation]]
- [[01 - NavHost and NavController]]
