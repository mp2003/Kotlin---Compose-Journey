---
up: "[[00 - MVI - Overview]]"
---

###### Elevator Pitch
- A six-step walkthrough that builds a small Task Manager screen using MVI, wiring State, Intent, Effect, ViewModel, and Compose end-to-end.

---

###### Definition
- A practice exercise that turns the MVI theory into a working screen with add and delete actions

---

###### Real-World Analogy
- Like assembling IKEA furniture
- Each step has one job
- Skip a step and the screen wobbles
- All six steps together = a complete unit

---

###### What
- A small screen that lists tasks
- User can add a task
- User can delete a task
- Toast appears when a task is deleted

---

###### Why
- Cements MVI by typing it out yourself
- Forces you to use State, Intent, Effect together
- Reveals weak spots before bigger screens

---

###### Core Concepts
- State -> data class -> StateFlow
- Intent -> sealed class
- Effect -> sealed class -> SharedFlow
- Update -> `.copy()`
- Emit -> `launch { emit() }`
- Entry -> `onIntent()`

---

###### Step 1 — Define State

Ask: "What does the UI show?"
- A list of tasks
- A loading flag

```kotlin
// Holds everything the screen needs to draw itself
data class TaskState(
    val tasks: List<String> = emptyList(),  // visible task list
    val isLoading: Boolean = false           // spinner flag
)
```

- Holds UI data only
- Always immutable
- Update with `.copy()`

---

###### Step 2 — Define Intent

Ask: "What can the user do?"
- Add a task
- Delete a task

```kotlin
// Closed set of user actions
sealed class TaskIntent {
    data class AddTask(val task: String) : TaskIntent()
    data class DeleteTask(val task: String) : TaskIntent()
}
```

- Only user actions
- No logic inside
- Just data

---

###### Step 3 — Define Effect

Ask: "What happens once?"
- Show a toast when a task is deleted

```kotlin
// One-time UI events
sealed class TaskEffect {
    data class ShowToast(val message: String) : TaskEffect()
}
```

- One-time event
- NOT stored in state
- Uses SharedFlow

---

###### Step 4 — ViewModel skeleton

```kotlin
class TaskViewModel : ViewModel() {

    // private holder, only ViewModel writes
    private val _state = MutableStateFlow(TaskState())
    val state = _state                 // public read-only

    // private emitter for one-time events
    private val _effect = MutableSharedFlow<TaskEffect>()
    val effect = _effect
}
```

- State -> StateFlow
- Effect -> SharedFlow
- Expose as read-only

---

###### Step 5 — Add `onIntent()`

Single entry point for every user action.

```kotlin
fun onIntent(intent: TaskIntent) {
    when (intent) {
        is TaskIntent.AddTask    -> addTask(intent.task)
        is TaskIntent.DeleteTask -> deleteTask(intent.task)
    }
}
```

- Single entry point
- No heavy logic here
- Just route the action

---

###### Step 6 — Handle the logic

```kotlin
// Pure state update, no side effect
private fun addTask(task: String) {
    _state.update {
        it.copy(tasks = it.tasks + task)
    }
}

// State update + one-time toast
private fun deleteTask(task: String) {
    _state.update {
        it.copy(tasks = it.tasks - task)
    }

    // 'launch' opens a coroutine; 'emit' sends one event
    viewModelScope.launch {
        _effect.emit(TaskEffect.ShowToast("Task Deleted"))
    }
}
```

- Add updates State only
- Delete updates State AND emits Effect

---

###### Memory Hook
- "Six steps: State, Intent, Effect, ViewModel, onIntent, Logic."

---

###### Key Rule
- Build in this order — every step depends on the one before it

---

###### Keywords Used

| Keyword | What it does |
|---------|--------------|
| `data class TaskState(...)` | Immutable object holding everything the screen needs to draw itself |
| `sealed class TaskIntent` | Closed set of user actions the ViewModel can receive |
| `sealed class TaskEffect` | Closed set of one-time UI events the ViewModel can emit |
| `data class AddTask(val task: String)` | Intent subclass carrying the text of the new task |
| `data class DeleteTask(val task: String)` | Intent subclass carrying the task to remove |
| `data class ShowToast(val message: String)` | Effect subclass carrying the toast text |
| `class TaskViewModel : ViewModel()` | ViewModel that holds state and processes intents |
| `MutableStateFlow(TaskState())` | Private writable state stream initialized with the default state |
| `MutableSharedFlow<TaskEffect>()` | Private one-time event stream |
| `fun onIntent(intent: TaskIntent)` | Single entry point routing all user actions |
| `when (intent)` | Exhaustive branch that dispatches each intent to its handler |
| `_state.update { it.copy(...) }` | Atomically replaces the state with a field-changed copy |
| `viewModelScope.launch { }` | Starts a coroutine scoped to the ViewModel to emit effects |
| `_effect.emit(...)` | Sends one event through the SharedFlow |

---

###### Related
- [[00 - MVI - Overview]]
- [[08 - Task Manager - Full Code]]
