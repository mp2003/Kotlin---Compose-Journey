---
up: "[[00 - Navigation - Overview]]"
---

###### Elevator Pitch
- Arguments are passed between screens by embedding them in the route string (`detail_screen/{name}/{age}`) and declaring their types with `navArgument` so Navigation parses them into the right Kotlin type.

---

###### Definition
- Route-based data passing: placeholders in the route path are filled at navigate time and read back via the destination's `arguments` bundle

---

###### Real-World Analogy
- Route template -> an envelope with blank lines for name and age
- `navigate(... withArgs)` -> filling in the blanks before posting
- `navArgument` -> the form rules ("age must be a number")
- `arguments?.getInt("age")` -> the recipient reading the filled form

---

###### What
- Route template -> `detail_screen/{name}/{age}` with named placeholders
- `navArgument("age") { type = NavType.IntType }` -> declares argument name + type
- `defaultValue` -> value used if none is supplied
- `nullable` -> whether the argument may be absent (Strings only can be nullable)
- `it.arguments?.getString("name")` -> reads the value inside the destination

---

###### Why
- Lets a screen be generic — `DetailScreen` works for any name/age
- Typed args (`NavType.IntType`) parse and validate the value for you
- Defaults make the route resilient when a value is missing

---

###### Core Concepts
- Placeholder `{name}` -> a slot in the route path filled at navigate time
- `NavType.StringType` / `NavType.IntType` -> declares how to parse the slot
- `defaultValue` -> fallback if the arg is not passed
- `nullable = true` -> only valid for String args, allows null
- `NavBackStackEntry.arguments` -> the Bundle holding parsed values
- Link: [[00 - Navigation - Overview]]

---

###### How it Works
- Define route as `route + "/{name}/{age}"`
- In `composable(...)` declare `arguments = listOf(navArgument(...) { })`
- To navigate, build the real path: `detail_screen/Milind/25`
- Navigation matches it against the template and parses each slot
- Inside the destination, read `it.arguments?.getInt("age")`

---

###### Syntax
- `route = ScreenState.DetailScreen.route + "/{name}/{age}"` -> template with slots
- `navArgument("name") { type = NavType.StringType; nullable = true }` -> declare arg
- `defaultValue = 0` -> fallback value
- `it.arguments?.getString("name")` -> read String arg
- `it.arguments?.getInt("age")` -> read Int arg
- `fun withArgs(vararg args: String)` -> helper that builds `route/arg1/arg2`

```kotlin
// Helper on the sealed route class
fun withArgs(vararg args: String): String = buildString {
    append(route)
    args.forEach { append("/$it") }            // -> "detail_screen/Milind/25"
}

composable(
    route = ScreenState.DetailScreen.route + "/{name}/{age}",
    arguments = listOf(
        navArgument("name") {
            type = NavType.StringType          // parse as String
            defaultValue = "Milind"            // used if missing
            nullable = true                    // String may be null
        },
        navArgument("age") {
            type = NavType.IntType             // parse as Int
            defaultValue = 0                   // used if missing
            nullable = false                   // Int cannot be nullable
        }
    )
) {
    DetailScreen(
        name = it.arguments?.getString("name"),
        age = it.arguments?.getInt("age"),
        onBack = { navController.popBackStack() }
    )
}
```

---

###### ASCII Flowchart

```text
[ navigate(DetailScreen.withArgs("Milind", "25")) ]
        |
        v   produces "detail_screen/Milind/25"
[ NavHost matches against "detail_screen/{name}/{age}" ]
        |
        v   navArgument types parse each slot
[ arguments Bundle: name="Milind", age=25 ]
        |
        v
[ DetailScreen(name, age) renders ]
```

---

###### Common Mistakes
- BAD: route template count != args passed -> no match, destination not found
- BAD: marking an `IntType` arg `nullable = true` -> only String args can be nullable
- BAD: passing an Int as the arg but reading with `getString` -> wrong/empty value
- BAD: forgetting `defaultValue` then navigating without the arg -> crash on missing key
- BAD: building the path by hand with typos -> route won't match the template

---

###### Weak Area Clarification
- Confusion: "Why pass age as a String in `withArgs` but read it as Int?"
- Why: the URL path is always text; the arg only becomes an Int after `NavType.IntType` parses it
- Resolution: you serialize to String for the path, Navigation deserializes back to the declared type on the other side

---

###### Common Follow-up Traps
- Q: Can an Int argument be nullable?
  A: No — only `NavType.StringType` supports `nullable = true`
- Q: What happens if I navigate without an argument that has a default?
  A: The `defaultValue` is used; without a default it crashes
- Q: Why does `withArgs` take `vararg String` if age is an Int?
  A: The route path is text; everything is appended as String, then parsed back by `navArgument`

---

###### Memory Hook
- Slots in the path, types in `navArgument`, defaults save you

---

###### Key Rule
- The route path is always text — `navArgument` types are what turn it back into real Kotlin types

---

###### Related
- [[01 - NavHost and NavController]]
- [[03 - Back Navigation]]
