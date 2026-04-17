- Responsible for updating state based on Intent
- Takes current state and user action as input
- Returns a new updated state
- Keeps state update logic separate from ViewModel
- Makes logic predictable and easier to manage

---
##### What
- A function that converts (oldState + intent) → newState

---

###### Why
- Avoids large and messy onIntent() functions
- Separates decision logic from state update logic
- Makes code easier to test and scale
- Keeps state updates consistent and centralized

---

###### Core Concepts
- Pure function (no side effects inside)
- Takes current state as input
- Takes intent/action as input
- Returns new state using copy()
- Does not mutate existing state

---

###### How it Works
- UI sends Intent
- ViewModel receives Intent
- ViewModel passes (state + intent) to Reducer
- Reducer returns new state
- ViewModel updates StateFlow
- UI re-renders

---

###### Weak Area Clarification (IMPORTANT)
- Reducer ONLY updates state
- Reducer should NOT:
  - call API
  - emit Effect
  - perform side effects
- Side effects are handled separately in ViewModel

---

###### Common Confusion
- "Reducer replaces ViewModel"
  → No, it only handles state updates
- "Reducer can do API calls"
  → Wrong, it must be pure
- "Reducer mutates state"
  → Wrong, always return new state

---

###### Common Mistakes
- Writing logic directly inside onIntent()
- Mutating state instead of using copy()
- Mixing side effects inside reducer
- Not using reducer for complex screens

---

###### Example (Kotlin)
```kotlin
fun reducer(state: UiState, intent: UiIntent): UiState {
    return when (intent) {

        is UiIntent.Add -> {
            state.copy(
                items = state.items + intent.item
            )
        }

        is UiIntent.Delete -> {
            state.copy(
                items = state.items - intent.item
            )
        }

        else -> state
    }
}
````

---

###### **Usage in ViewModel**

```kotlin
fun onIntent(intent: UiIntent) {
    _state.update { currentState ->
        reducer(currentState, intent)
    }
}
```

---

###### **Related Concepts**

[[MVI Architecture]]  
[[State]]  
[[Intent]]  
[[onIntent]]  
[[ViewModel]]