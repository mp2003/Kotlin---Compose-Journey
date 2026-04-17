# Effect

- Represents one-time UI actions (happens once)
- Not part of UI state (UI should not remember it)
- Triggered by ViewModel after some action
- UI reacts once and does not repeat it
- Does not re-trigger on recomposition

---

##### What
- A one-time event for UI reaction

---

###### Why
- Keeps state clean (no mixing with events)
- Prevents repeated actions (like multiple toasts on recomposition)
- Separates UI display (state) from UI actions (effect)

---

###### Core Concepts
- Defined as sealed class
- Emitted using SharedFlow (NOT StateFlow)
- Collected in UI using LaunchedEffect
- Never stored inside State

---

###### How it Works
- User performs action
- ViewModel processes it
- ViewModel emits Effect
- UI collects Effect once
- UI performs action (toast/navigation)

---

###### Weak Area Clarification (IMPORTANT)
- Toast → Effect (happens once)
- Navigation → Effect (one-time action)
- Snackbar → Effect

- Dialog:
  - Show dialog once → Effect
  - Dialog visible on screen → State

- Rule:
  - If UI should remember → State
  - If UI should happen once → Effect

---

###### Common Confusion
- "Error message is always Effect"  
  → If shown on screen → State

- "Everything from ViewModel is State"  
  → One-time things are Effects

- "Effect can be stored"  
  → Should NOT be stored

- "StateFlow can be used for Effect"  
  → Causes repeated triggers

---

###### Common Mistakes
- Putting toast/navigation inside State
- Using StateFlow instead of SharedFlow
- Re-triggering effects on recomposition
- Treating Effect as UI data

---

###### Example (Kotlin)
```kotlin
sealed class UiEffect {
    data class ShowToast(val message: String)
    data class Navigate(val route: String)
}
```


###### **Related Concepts**
[[MVI Architecture]]  
[[State]]  
[[Intent]]  
[[SharedFlow]]