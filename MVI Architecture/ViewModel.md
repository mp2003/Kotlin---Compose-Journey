###### Explanation 
- Acts as the central decision maker
- Receives user actions (Intent)
- Updates UI data (State)
- Emits one-time events (Effect)
- UI never contains business logic

###### What
- A layer that handles logic and manages State & Effect

###### Why
- Separates UI from business logic
- Centralizes all decisions
- Makes app predictable and testable

###### Core Concepts
- Holds State (StateFlow)
- Emits Effect (SharedFlow)
- Processes Intent via onIntent()
- Contains business logic only

###### Flow / How it Works
- UI sends Intent
- ViewModel receives Intent
- ViewModel processes logic
- ViewModel updates State
- ViewModel emits Effect (if needed)
- UI reacts to State and Effect

## Example (Kotlin)

```kotlin
class TaskViewModel : ViewModel() {

    private val _state = MutableStateFlow(UiState())
    val state = _state

    private val _effect = MutableSharedFlow<UiEffect>()
    val effect = _effect

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
}
```

###### ❌ Common Mistake

- Calling ViewModel functions directly from UI

```kotlin
viewModel.addTask("Task")
````

###### **Why It’s Wrong**

- Breaks **Unidirectional Data Flow**
- UI controls business logic (not allowed in MVI)
- Turns architecture closer to MVVM instead of true MVI

---

###### **Solution**

- Always send **Intent** from UI

```kotlin
viewModel.onIntent(UiIntent.Add("Task"))
```

---

###### Common Mistake

- Mutating state directly inside ViewModel

```kotlin
fun onIntent(intent: UiIntent) {
    when(intent) {
        is UiIntent.Add -> _state.value.items.add(intent.item)
    }
}
```


###### **Why It’s Wrong**

- Breaks **immutability**
- `StateFlow` may NOT emit updates properly
- UI might not recompose/update
- Leads to unpredictable bugs

---

###### ** Solution**

- Always create a **new state copy**

```kotlin
fun onIntent(intent: UiIntent) {
    when(intent) {
        is UiIntent.Add -> {
            _state.update {
                it.copy(items = it.items + intent.item)
            }
        }
    }
}
```

###### **Rule**

Never mutate state  
Always replace state with a new copy