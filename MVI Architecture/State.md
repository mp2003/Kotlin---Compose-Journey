- Represents what UI displays
- Contains all data required for screen
- UI reads state and renders
- State is immutable
- Changes in state trigger recomposition

###### What
- A data holder representing current UI condition

###### Why
- Keeps UI predictable
- Centralizes UI data
- Avoids scattered logic

###### Core Concepts
- Single state per screen
- Immutable → use copy()
- Holds only UI data
- Observed via StateFlow

###### How it Works

- ViewModel holds state
- UI observes state
- State updates
- UI re-renders

###### Example (Kotlin)

```kotlin
data class UiState(
    val items: List<String> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null
)
