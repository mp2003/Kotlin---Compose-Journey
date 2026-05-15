---
up: "[[00 - Navigation - Overview]]"
---

###### Elevator Pitch
- Back navigation pops the current screen off the back stack with `popBackStack()`, and the clean way to wire it is to hoist it as an `onBack: () -> Unit` lambda so the screen never touches the `NavController`.

---

###### Definition
- Returning to the previous destination by removing the top entry of the back stack, exposed to the screen as a hoisted callback

---

###### Real-World Analogy
- Back stack -> a pile of plates
- `popBackStack()` -> taking the top plate off
- Hoisted `onBack` lambda -> a doorbell: the screen rings it, someone else decides what happens

---

###### What
- `navController.popBackStack()` -> removes the top screen, shows the one below
- Hoisted lambda -> screen takes `onBack: () -> Unit`, not `NavController`
- The graph supplies `onBack = { navController.popBackStack() }`
- Result -> screen is decoupled, previewable, testable

---

###### Why
- Screen with `NavController` is coupled to the Navigation library, hard to `@Preview`/test
- Screen with `onBack: () -> Unit` is a pure function — pass `{}` in previews
- Same state-hoisting / UDF principle applied to navigation events

---

###### Core Concepts
- `popBackStack()` -> pop one entry off the back stack
- `navigateUp()` -> similar, respects parent navigation (used with up button)
- Hoisting -> lift the navigation decision OUT of the screen into the graph
- Decoupling -> screen knows nothing about how "back" is implemented
- Link: [[00 - Navigation - Overview]]

---

###### How it Works
- Screen declares `onBack: () -> Unit` parameter
- Screen's button does `onClick = onBack` — nothing more
- In `NavHost`, the destination passes `onBack = { navController.popBackStack() }`
- Tap -> lambda fires -> controller pops the stack -> previous screen shows

---

###### Syntax
- `navController.popBackStack()` -> pop top of back stack
- `navController.navigateUp()` -> go up one level (toolbar up button)
- `onBack: () -> Unit` -> hoisted callback parameter on the screen
- `Button(onClick = onBack) { Text("Go Back") }` -> screen just rings the bell

```kotlin
// Screen — knows NOTHING about navigation
@Composable
fun DetailScreen(
    name: String?,
    age: Int?,
    onBack: () -> Unit                       // hoisted callback
) {
    Column {
        Text("Hello $name your age is $age")
        Button(onClick = onBack) {           // just calls the lambda
            Text("Go Back")
        }
    }
}

// Graph — owns the wiring
DetailScreen(
    name = it.arguments?.getString("name"),
    age = it.arguments?.getInt("age"),
    onBack = { navController.popBackStack() } // decision lives here
)
```

---

###### Layered Diagram

```text
+-------------------------------+
|  DetailScreen(onBack)         |  pure UI, no NavController
+-------------------------------+
|  NavHost composable block      |  onBack = { popBackStack() }
+-------------------------------+
|  NavController + back stack     |  actually pops the entry
+-------------------------------+
```

---

###### Common Mistakes
- BAD: passing `NavController` into the screen just to call `popBackStack()` -> coupling
- BAD: calling `popBackStack()` on the start destination -> nothing to pop, may exit app unexpectedly
- BAD: confusing `popBackStack()` (pop one) with `popUpTo()` (pop many) -> see note 05
- BAD: doing navigation logic inside the screen body instead of in a lambda -> not testable

---

###### Weak Area Clarification
- Confusion: "Why not just pass NavController, it's less code?"
- Why: it works, but the screen now depends on Navigation — can't preview, can't unit test, reusable nowhere
- Resolution: hoist the event; the screen stays a dumb function, the graph decides behavior

---

###### Common Follow-up Traps
- Q: `popBackStack()` vs `navigateUp()`?
  A: `popBackStack()` pops the current entry; `navigateUp()` respects the navigation hierarchy (used by the toolbar up arrow)
- Q: What does `popBackStack()` return?
  A: A Boolean — false if the back stack was already at the start (nothing popped)
- Q: Why hoist instead of using `NavController` directly?
  A: Same reason you hoist state — the screen becomes pure, previewable, and testable

---

###### Memory Hook
- The screen rings the bell (`onBack`); the graph answers the door (`popBackStack`)

---

###### Key Rule
- Never give a screen the `NavController` — give it `onBack: () -> Unit` and wire it in the graph

---

###### Related
- [[00 - Navigation - Overview]]
- [[05 - Nested Graphs]]
