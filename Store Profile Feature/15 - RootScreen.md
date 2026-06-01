---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- RootScreen stateful host hai jo ViewModel se baat karta hai, effects collect karta hai, aur InitialState ke hisaab se UI dikhata hai.

---

###### What
- File: `presentation/screen/StoreProfileRootScreen.kt`
- `hiltViewModel()` se VM leta hai
- Flows ko `collectAsStateWithLifecycle()` se collect
- `LaunchedEffect` me effects collect -> toast local, nav upar
- `Crossfade(initialState)` -> Loading/Error/Success

---

###### Why
- Ye "impure" half hai -> ViewModel, lifecycle, effects, nav sab yahan
- Isse pure Screen alag aur testable rehti hai

---

###### How to write it (soch)
1. VM aur `onNavigationChange` param lo
2. `collectAsStateWithLifecycle` se 3 flows collect
3. `LaunchedEffect(Unit)` me `effect.collectLatest { when() }`
4. `Crossfade(initialState)` -> har case ka UI

```kotlin
@Composable
fun StoreProfileRootScreen(
    viewModel: StoreProfileViewModel = hiltViewModel(),
    onNavigationChange: (StoreProfileNavigation) -> Unit = {},
) {
    val initial by viewModel.initialState.collectAsStateWithLifecycle()
    val ui by viewModel.uiState.collectAsStateWithLifecycle()

    LaunchedEffect(Unit) {                         // effects = one-shot
        viewModel.effect.collectLatest {
            when (it) {
                is ShowToast -> { /* show toast */ }
                NavigateBack -> onNavigationChange(NavigateBack) // forward nav
            }
        }
    }

    Crossfade(initial) { when (it) {               // first-load UI switch
        Loading -> { /* spinner */ }
        is Error -> ErrorScreen(...)
        Success -> StoreProfileScreen(uiState = ui, onBack = { ... })
    } }
}
```

---

###### Common Mistakes
- BAD: SharedFlow ko composable body me seedha collect karna
- BAD: NavController yahan ghusana (Destination ka kaam)

---

###### Memory Hook
- "Root = gandagi (VM, effects, nav) yahan; pure Screen saaf rahe."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `hiltViewModel()` | Hilt se VM leta hai |
| `collectAsStateWithLifecycle()` | Flow ko safely (lifecycle-aware) collect |
| `LaunchedEffect(Unit)` | Composition pe ek baar coroutine chalao |
| `collectLatest` | Latest event collect, purana cancel |
| `Crossfade` | States ke beech fade animation |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[16 - Pure Screen]]
- [[13 - InitialState]]
