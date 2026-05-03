---
up: "[[MVI - Overview]]"
---

###### Elevator Pitch
- A Reducer is a pure function that takes the current State plus an Intent and returns the new State, with no side effects.

---

###### Definition
- A function with the shape `(state, intent) -> newState`

---

###### Real-World Analogy
- Like a cashier at a checkout
- You hand over a receipt (state) and a new item (intent)
- Cashier returns a new updated receipt
- Cashier never calls the supplier or rings the doorbell — that is someone else's job

---

###### What
- Updates state based on Intent
- Takes (current state + intent) as input
- Returns a new state
- Keeps update logic separate from ViewModel

---

###### Why
- Avoids huge `onIntent()` functions
- Splits "decide" from "do"
- Easy to unit-test (pure function)
- Keeps state updates consistent

---

###### Core Concepts
- Pure function — no side effects
- Inputs: current state + intent
- Output: new state via `.copy()`
- Never mutates the old state
- Never calls APIs or emits Effects

---

###### How it Works
- UI sends Intent
- ViewModel receives it
- ViewModel calls `reducer(state, intent)`
- Reducer returns new state
- ViewModel updates StateFlow
- UI re-renders

---

###### Example

```kotlin
// Pure function: same input always gives same output
fun reducer(state: UiState, intent: UiIntent): UiState {
    return when (intent) {

        is UiIntent.Add -> {
            // new state with the new item appended
            state.copy(items = state.items + intent.item)
        }

        is UiIntent.Delete -> {
            // new state with the item removed
            state.copy(items = state.items - intent.item)
        }

        else -> state   // no change
    }
}
```

**Used inside ViewModel:**
```kotlin
fun onIntent(intent: UiIntent) {
    _state.update { current ->
        reducer(current, intent)
    }
}
```

---

###### Weak Area Clarification
- Reducer ONLY updates state
- Reducer does NOT call APIs
- Reducer does NOT emit Effects
- Side effects belong in the ViewModel, not here

---

###### Common Mistakes
- BAD: Writing logic directly inside `onIntent` and skipping the reducer
- BAD: Mutating state instead of using `.copy()`
- BAD: Calling network or DB code inside the reducer
- BAD: Emitting Effects from the reducer

---

###### Common Follow-up Traps
- Q: Does every screen need a reducer?
  A: No — small screens can do it inline; reducers shine when state logic is complex.
- Q: Why must it be pure?
  A: Predictable, testable, and easy to reason about.
- Q: Reducer vs `onIntent`?
  A: `onIntent` routes; reducer transforms state.

---

###### Memory Hook
- "Reducer = cashier. Old receipt + new item = new receipt."

---

###### Key Rule
- Pure function. State in, state out. Nothing else.

---

###### Related
- [[MVI - Overview]]
- [[State - UI Data]]
- [[onIntent - Entry Point]]
