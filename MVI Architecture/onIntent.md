- Entry point for all user actions from UI
- UI sends Intent instead of calling multiple functions
- ViewModel receives and decides what to do
- Routes action to correct logic or reducer
- Keeps flow centralized and predictable

---

###### What
- A function that handles all user Intents in [[ViewModel]]

---

###### Why
- Avoids multiple public functions in ViewModel
- Centralizes all user actions in one place
- Makes debugging and tracking easier
- Keeps UI simple (UI only sends Intent)

---

###### Core Concepts
- Single entry point function
- Accepts UiIntent as parameter
- Uses when(intent) to decide action
- Can call reducer or internal functions
- Updates State or emits Effect

---

###### How it Works
- UI sends Intent
- onIntent() receives it
- when(intent) decides what to do
- ViewModel processes logic
- State is updated OR Effect is emitted
- UI reacts to changes

---

###### Weak Area Clarification (IMPORTANT)
- onIntent is NOT business logic
  → it only routes actions

- Heavy logic should NOT be written here
  → move to reducer or separate functions

- onIntent does NOT update UI directly
  → it updates State or emits Effect

---

###### Common Confusion
- "onIntent is same as function call"
  → No, it is a centralized entry point

- "UI can call ViewModel functions directly"
  → In MVI, UI should ONLY call onIntent()

- "All logic should be inside onIntent"
  → Keep it small, delegate logic

---

###### Common Mistakes
- Writing large when(intent) blocks
- Putting business logic directly inside onIntent
- Creating multiple public functions instead of one entry point
- Updating UI directly from ViewModel

---

###### Example (Kotlin)
```kotlin
fun onIntent(intent: UiIntent) {
    when (intent) {

        is UiIntent.Add -> {
            _state.update {
                it.copy(items = it.items + intent.item)
            }
        }

        is UiIntent.Delete -> {
            _state.update {
                it.copy(items = it.items - intent.item)
            }
        }
    }
}