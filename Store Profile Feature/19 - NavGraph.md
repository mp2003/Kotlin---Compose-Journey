---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- NavGraph feature ka ek public function hai jise landing host call karke poora feature plug kar leta hai.

---

###### What
- File: `presentation/navigation/StoreProfileNavGraph.kt`
- Public extension: `fun NavGraphBuilder.storeProfileNavGraph(navController)`
- Andar `FeatureNavigationGraph` -> `nestedGraph` me destinations add

---

###### Why
- Feature ka ek hi public navigation door
- Host sirf ye function bulata, andar ki screens nahi jaanta

---

###### How to write it (soch)
1. Ek `NavGraphBuilder` extension banao (feature ka entry)
2. `featureRoute` = Graph, `startDestination` = first screen
3. `nestedGraph` ke andar `addDestinations(...)` se screens daalo

```kotlin
fun NavGraphBuilder.storeProfileNavGraph(navController: NavController) {
    StoreProfileNavigationGraph(navController).build(this)
}

private class StoreProfileNavigationGraph(
    val navController: NavController,
) : FeatureNavigationGraph() {
    override val featureRoute = StoreProfileRoutes.Graph.route
    override val startDestination = StoreProfileRoutes.StoreProfile.route
    override fun NavGraphBuilder.addFeatureGraph() {
        nestedGraph(featureRoute, startDestination) {
            addDestinations(StoreProfileDestination(navController))
        }
    }
}
```

---

###### Host me kaise lagta hai (aakhri wiring — abhi pending)
- `LandingScreen.kt` ke `NavigationHost { }` me: `storeProfileNavGraph(navController.navController)`
- Drawer item ka `onClick`: `onNavigateTo(StoreProfileRoutes.Graph.route)`

---

###### Common Mistakes
- BAD: host ko feature ki internal screens se depend karana
- BAD: `startDestination` galat route dena

---

###### Memory Hook
- "NavGraph = feature ka single plug. Host ek hi function dabaye."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `NavGraphBuilder.func()` | Nav graph pe extension (feature entry) |
| `FeatureNavigationGraph` | Feature graph ka base class |
| `nestedGraph` | Ek scoped sub-graph banata |
| `addDestinations` | Multiple destinations register |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[05 - Nested Graphs]]
- [[18 - Destinations]]
