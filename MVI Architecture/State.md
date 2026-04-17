- Represents what UI displays on screen
- Contains all data needed to draw UI
- UI only reads state (does not change it directly)
- State should always be immutable
- When state changes → UI automatically updates (recomposition)

###### What
- A data object that represents the current UI condition

###### Why
- Keeps UI predictable and easy to debug
- All UI data comes from one place
- Avoids random UI updates from different places

###### Core Concepts
- Single state per screen (single source of truth)
- Immutable → never modify directly, always create new state
- Use copy() to update values
- Holds only UI data (NOT events like toast/navigation)
- Observed using StateFlow

###### How it Works
- ViewModel holds the state
- UI observes state (collects StateFlow)
- User action happens → state is updated in ViewModel
- New state is emitted
- UI re-renders based on new state

###### Example (Kotlin)
```kotlin
data class UiState(
    val items: List<String> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null
)
````

###### **Update State (Correct Way)**

```kotlin
_state.update {
    it.copy(items = it.items + "New Item")
}
```

###### **Wrong Way (DO NOT DO)**

```kotlin
_state.value.items.add("New Item") // ❌ mutating state
```

###### **Key Points**

- State = what UI shows (and stays)
- Always immutable
- Never store one-time events in state
- UI should not modify state directly

###### **Common Mistakes**

- Putting toast/navigation inside state ❌
- Mutating list directly ❌
- Having multiple state objects ❌
- Thinking state = events ❌

###### **Related Concepts**

[[MVI Architecture]]  
[[Intent]]  
[[Effect]]  
[[StateFlow]]

---
