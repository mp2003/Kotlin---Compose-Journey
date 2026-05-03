###### Elevator Pitch
- Effect is a one-time UI event like a toast or navigation that the ViewModel fires once and the UI reacts to without storing it.

---

###### Real-World Analogy
- Like a doorbell ringing once
- You hear it, you act
- It does not keep ringing every time you walk past the door

---

###### Definition
- A sealed class emitted via SharedFlow that represents a one-time UI action

---

###### What
- Represents a one-time event (toast, snackbar, navigate)
- NOT part of UI state
- Triggered by the ViewModel
- UI reacts once and forgets

---

###### Why
- Keeps State clean of event noise
- Prevents repeated actions on recomposition (no double toasts)
- Separates "what UI shows" (State) from "what UI does once" (Effect)

---

###### Core Concepts
- Defined as sealed class
- Emitted using SharedFlow (NOT StateFlow)
- Collected with LaunchedEffect in Compose
- Never stored inside State

---

###### How it Works
- User triggers an action
- ViewModel processes it
- ViewModel emits an Effect
- UI collects the Effect once
- UI performs the action (toast, navigate)

---

###### Example

```kotlin
// One-time events the UI should react to once
sealed class UiEffect {
    data class ShowToast(val message: String) : UiEffect()
    data class Navigate(val route: String) : UiEffect()
}
```

**Emit from ViewModel:**
```kotlin
// 'launch' opens a coroutine; 'emit' sends one event
viewModelScope.launch {
    _effect.emit(UiEffect.ShowToast("Saved"))
}
```

**Collect in Compose:**
```kotlin
// LaunchedEffect collects events, runs once per emission
LaunchedEffect(Unit) {
    viewModel.effect.collect { effect ->
        when (effect) {
            is UiEffect.ShowToast -> showToast(effect.message)
            is UiEffect.Navigate  -> navigate(effect.route)
        }
    }
}
```

---

###### Weak Area Clarification
- Toast -> Effect (happens once)
- Navigation -> Effect (one-time action)
- Snackbar -> Effect
- Dialog visible on screen -> State
- Dialog "show once" trigger -> Effect
- Rule: if UI should remember -> State; if UI should act once -> Effect

---

###### Common Mistakes
- BAD: Putting toast/navigation inside State
- BAD: Using StateFlow instead of SharedFlow
- BAD: Re-triggering effects on recomposition
- BAD: Storing the effect somewhere "for later"

---

###### Common Follow-up Traps
- Q: Why SharedFlow not StateFlow?
  A: StateFlow re-emits on collect, causing duplicate toasts.
- Q: Is "error message" a State or Effect?
  A: State if shown on screen permanently, Effect if shown once.
- Q: Can an Effect be stored?
  A: No — that defeats the "one-time" guarantee.

---

###### Memory Hook
- "Effect = doorbell. Rings once, no echo."

---

###### Key Rule
- If UI should remember it -> State. If UI should act once -> Effect.

---

###### Related
- [[MVI Architecture]]
- [[State]]
- [[Intent]]
- [[ViewModel]]
