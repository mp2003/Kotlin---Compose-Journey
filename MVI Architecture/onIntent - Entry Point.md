###### Elevator Pitch
- `onIntent()` is the single front door of the ViewModel — every user action enters here and gets routed to the right logic.

---

###### Definition
- A function inside the ViewModel that takes a `UiIntent` and decides what to do with it

---

###### Real-World Analogy
- Like a hotel reception desk
- Every guest request goes through reception
- Reception does not cook the food or clean the room
- It just routes the request to the right department

---

###### What
- Single entry point for all user actions
- Receives one Intent at a time
- Uses `when(intent)` to route
- Calls reducer or internal helpers

---

###### Why
- Avoids dozens of public ViewModel functions
- Centralizes all user actions
- Makes logging and debugging trivial
- Keeps UI dumb — it only calls `onIntent()`

---

###### Core Concepts
- Single function, single parameter
- Routes via `when(intent)`
- Updates State or emits Effect
- Heavy logic goes elsewhere (reducer or private helpers)

---

###### How it Works
- UI sends Intent
- `onIntent()` receives it
- `when(intent)` picks the matching branch
- ViewModel updates State or emits Effect
- UI reacts

---

###### Example

```kotlin
fun onIntent(intent: UiIntent) {
    when (intent) {

        is UiIntent.Add -> {
            // delegates to a private function
            addItem(intent.item)
        }

        is UiIntent.Delete -> {
            deleteItem(intent.item)
        }
    }
}

// Private helpers keep onIntent slim
private fun addItem(item: String) {
    _state.update { it.copy(items = it.items + item) }
}

private fun deleteItem(item: String) {
    _state.update { it.copy(items = it.items - item) }
}
```

---

###### Weak Area Clarification
- `onIntent()` is NOT business logic — it only routes
- Heavy logic should NOT live inside `when` branches
- `onIntent()` does NOT update UI directly — it updates State or emits Effect

---

###### Common Mistakes
- BAD: Writing huge logic blocks inside each `when` branch
- BAD: Creating multiple public functions instead of one entry point
- BAD: Updating UI directly from `onIntent`
- BAD: Forgetting to handle a new Intent (sealed class will warn you)

---

###### Common Follow-up Traps
- Q: Why one entry point instead of multiple methods?
  A: Easier to log, replay, and test — every action funnels through one place.
- Q: Should `onIntent` be `suspend`?
  A: No — keep it sync; launch coroutines inside branches if needed.
- Q: Can a single Intent trigger State + Effect?
  A: Yes — update State, then emit Effect inside the same branch.

---

###### Memory Hook
- "onIntent = reception desk. Route, never cook."

---

###### Key Rule
- Route in `onIntent`, do the work elsewhere

---

###### Related
- [[MVI - Overview]]
- [[Intent - User Actions]]
- [[Reducer - Pure State Update]]
