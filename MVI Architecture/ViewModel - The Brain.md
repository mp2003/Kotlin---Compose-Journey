---
up: "[[MVI - Overview]]"
---

###### Elevator Pitch
- ViewModel is the brain of the screen — it receives Intents, updates State, emits Effects, and never lets UI touch business logic.

---

###### Definition
- A class that holds the screen's State and Effect streams and processes Intents through a single `onIntent()` entry point

---

###### Real-World Analogy
- Like a restaurant kitchen
- Waiter (UI) takes orders (Intents)
- Kitchen (ViewModel) cooks (logic)
- Plates the food (State) and rings the bell once (Effect)
- Customers never enter the kitchen

---

###### What
- Receives user actions (Intent)
- Updates UI data (State)
- Emits one-time events (Effect)
- Holds business logic (UI never does)

---

###### Why
- Separates UI from logic
- Centralizes all decisions in one place
- Survives configuration changes (rotation)
- Makes testing easy — no UI needed

---

###### Core Concepts
- Holds State as `StateFlow`
- Emits Effect as `SharedFlow`
- Processes Intent via `onIntent()`
- Contains business logic only — no UI code

---

###### How it Works
- UI sends Intent
- ViewModel receives via `onIntent()`
- ViewModel runs logic
- ViewModel updates State
- ViewModel emits Effect if needed
- UI reacts to State and Effect

---

###### Example

```kotlin
class TaskViewModel : ViewModel() {

    // private holder, only ViewModel can change
    private val _state = MutableStateFlow(UiState())
    // public read-only, UI observes this
    val state = _state

    // private emitter for one-time events
    private val _effect = MutableSharedFlow<UiEffect>()
    val effect = _effect

    // single entry point for all user actions
    fun onIntent(intent: UiIntent) {
        when (intent) {
            is UiIntent.Add -> {
                _state.update { it.copy(items = it.items + intent.item) }
            }
            is UiIntent.Delete -> {
                _state.update { it.copy(items = it.items - intent.item) }
            }
        }
    }
}
```

---

###### Common Mistakes
- BAD: Calling `viewModel.addTask(...)` directly from UI -> breaks unidirectional flow
- BAD: Mutating state with `_state.value.items.add(...)` -> StateFlow may not emit
- BAD: Doing UI work inside ViewModel (e.g. building Toast objects)

**Wrong:**
```kotlin
viewModel.addTask("Task")   // BAD: skips Intent
```

**Right:**
```kotlin
viewModel.onIntent(UiIntent.Add("Task"))   // OK: goes through Intent
```

---

###### Common Follow-up Traps
- Q: Why one `onIntent()` entry point?
  A: Centralizes all actions, easier to log, debug, and test.
- Q: Why expose `StateFlow` instead of `LiveData`?
  A: Coroutine-native and works on any platform.
- Q: Should ViewModel know about Compose?
  A: No — it stays UI-framework-free.

---

###### Memory Hook
- "ViewModel = kitchen. Waiter brings orders, kitchen cooks, customers wait outside."

---

###### Key Rule
- UI sends Intent; ViewModel decides everything else

---

###### Related
- [[MVI - Overview]]
- [[onIntent - Entry Point]]
- [[Effect - One-time Events]]
