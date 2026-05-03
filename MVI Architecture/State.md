###### Elevator Pitch
- State is a single immutable object that describes everything the UI is currently showing on screen.

---

###### Definition
- A data class held by the ViewModel that represents the current UI condition

---

###### Real-World Analogy
- Like a photograph of the screen at this exact moment
- Take a new photo to update -> never edit the old one
- UI looks at the latest photo and redraws

---

###### What
- Holds all data needed to draw the UI
- Always immutable
- Updated by replacing, not mutating
- Observed by UI through StateFlow

---

###### Why
- Keeps UI predictable
- Single source of truth for what's on screen
- Makes debugging easy — just print the state
- Prevents random updates from random places

---

###### Core Concepts
- One State per screen
- Immutable -> use `.copy()` to update
- Holds UI data only (NOT one-time events)
- Exposed via StateFlow
- UI reads State, never writes to it

---

###### How it Works
- ViewModel holds the State
- UI collects the StateFlow
- User action triggers an update
- ViewModel emits new State (via copy)
- UI recomposes with the new State

---

###### Example

```kotlin
// One data class = the whole screen's UI condition
data class UiState(
    val items: List<String> = emptyList(), // what the list shows
    val isLoading: Boolean = false,        // spinner flag
    val error: String? = null              // error text or null
)
```

**Correct update:**
```kotlin
// '.update { ... }' replaces the value safely
// '.copy(...)' creates a NEW state with one field changed
_state.update {
    it.copy(items = it.items + "New Item")
}
```

**Wrong update:**
```kotlin
// BAD: mutates the existing list, UI may not recompose
_state.value.items.add("New Item")
```

---

###### Common Mistakes
- BAD: Putting toast or navigation inside State (those are Effects)
- BAD: Mutating the list directly
- BAD: Having multiple State objects for one screen
- BAD: Treating State like a stream of events

---

###### Common Follow-up Traps
- Q: Why immutable?
  A: Prevents hidden mutations and lets StateFlow detect changes correctly.
- Q: Should error messages live in State or Effect?
  A: State if shown on screen, Effect if shown once as a toast.
- Q: Why one State object instead of many?
  A: Single source of truth — easier to reason about and test.

---

###### Memory Hook
- "State is the photo, never the brush."

---

###### Key Rule
- Never mutate — always replace with `.copy()`

---

###### Related
- [[MVI Architecture]]
- [[Intent]]
- [[Effect]]
- [[ViewModel]]
- [[Reducer]]
