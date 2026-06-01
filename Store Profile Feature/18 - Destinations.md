---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Destination wo jagah hai jahan ek route ko uski screen se joda jaata hai, aur navigation commands ko NavController calls me badla jaata hai.

---

###### What
- File: `presentation/navigation/StoreProfileDestinations.kt`
- `class StoreProfileDestination(navController) : BaseDestination()`
- `route` = screen route, `screen` = RootScreen with `onNavigationChange`

---

###### Why
- ==Sirf yahi jagah NavController ko chhooti hai==
- Abstract command (`NavigateBack`) ko real call (`popBackStack()`) me badalta

---

###### How to write it (soch)
1. `BaseDestination` extend karo (free transitions milti)
2. `route` override -> kaun sa route
3. `screen` override -> RootScreen daalo
4. `onNavigationChange` me `when(nav)` -> NavController call

```kotlin
class StoreProfileDestination(val navController: NavController) : BaseDestination() {
    override val route = StoreProfileRoutes.StoreProfile
    override val screen: @Composable (...) = {
        StoreProfileRootScreen(
            onNavigationChange = { nav ->
                when (nav) {
                    NavigateBack -> navController.popBackStack() // command -> real call
                }
            }
        )
    }
}
```

---

###### Common Mistakes
- BAD: NavController ko Screen/ViewModel me le jaana
- BAD: `when(nav)` me case chhodna (exhaustive todna)

---

###### Memory Hook
- "Destination = route + screen ka jodi, aur NavController ka akela maalik."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `BaseDestination` | Route+screen ka base, default transitions |
| `override` | Base ke property/method ko define karo |
| `popBackStack()` | Pichli screen pe wapas |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[12 - Navigation]]
- [[03 - Back Navigation]]
