#### **STEP 1 — DEFINE STATE**

👉 Ask:  “What does UI show?”
- Answer:
	- List of tasks
	- Loading

``` kotlin
data class TaskState(
    val tasks: List<String> = emptyList(),
    val isLoading: Boolean = false
)
```

###### **Remember**
- Holds UI data
- Always immutable
- Use copy() to update
---

#### **STEP 2 — DEFINE INTENT**

👉 Ask:  “What can user do?”
- Answer:
	- Add task
	- Delete task

``` kotlin
sealed class TaskIntent {
    data class AddTask(val task: String) : TaskIntent()
    data class DeleteTask(val task: String) : TaskIntent()
}
```

###### **Remember**
- Only user actions
- No logic inside
- Just data
---
#### **STEP 3 — DEFINE EFFECT**

👉 Ask:   “What happens once?”
- Answer:
	- Show toast

```kotlin
sealed class TaskEffect {
    data class ShowToast(val message: String) : TaskEffect()
}
```

###### **Remember**

- One-time event
- NOT stored in state
- Uses SharedFlow

---

#### **STEP 4 — CREATE VIEWMODEL (BASIC STRUCTURE)**

```kotlin 
class TaskViewModel : ViewModel() {
    private val _state = MutableStateFlow(TaskState())
    val state = _state

    private val _effect = MutableSharedFlow<TaskEffect>()
    val effect = _effect
}
```

###### **Remember**

- State → StateFlow
- Effect → SharedFlow
- Expose as read-only (later improvement)

---

##### **STEP 5 — ADD onIntent()**

👉 This is your entry point

```kotlin 
fun onIntent(intent: TaskIntent) {
    when (intent) {

        is TaskIntent.AddTask -> {
            addTask(intent.task)
        }

        is TaskIntent.DeleteTask -> {
            deleteTask(intent.task)
        }
    }
}
```

###### **Remember**

- Single entry point
- Do NOT write heavy logic here
- Just route actions

---

#### STEP 6 - Handle Logic 

```kotlin
private fun addTask(task: String) {
    _state.update {
        it.copy(tasks = it.tasks + task)
    }
}

private fun deleteTask(task: String) {
    _state.update {
        it.copy(tasks = it.tasks - task)
    }

    viewModelScope.launch {
        _effect.emit(TaskEffect.ShowToast("Task Deleted"))
    }
}

```


---

###### Points to remember
	State  → data class → StateFlow
	Intent → sealed class
	Effect → sealed class → SharedFlow

	Update → copy()
	Emit   → launch { emit() }
	Entry  → onIntent()



##### Full Code

```kotlin
# TaskViewModel.kt

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.MutableSharedFlow
import kotlinx.coroutines.flow.update
import kotlinx.coroutines.launch

// ----------------------
// STATE
// ----------------------
data class TaskState(
    val tasks: List<String> = emptyList(),
    val isLoading: Boolean = false
)

// ----------------------
// INTENT
// ----------------------
sealed class TaskIntent {
    data class AddTask(val task: String) : TaskIntent()
    data class DeleteTask(val task: String) : TaskIntent()
}

// ----------------------
// EFFECT
// ----------------------
sealed class TaskEffect {
    data class ShowToast(val message: String) : TaskEffect()
}

// ----------------------
// VIEWMODEL
// ----------------------
class TaskViewModel : ViewModel() {

    // StateFlow (UI State)
    private val _state = MutableStateFlow(TaskState())
    val state = _state

    // SharedFlow (One-time events)
    private val _effect = MutableSharedFlow<TaskEffect>()
    val effect = _effect

    // Entry point for all user actions
    fun onIntent(intent: TaskIntent) {
        when (intent) {

            is TaskIntent.AddTask -> {
                addTask(intent.task)
            }

            is TaskIntent.DeleteTask -> {
                deleteTask(intent.task)
            }
        }
    }

    // ----------------------
    // INTERNAL LOGIC
    // ----------------------

    private fun addTask(task: String) {
        _state.update {
            it.copy(tasks = it.tasks + task)
        }
    }

    private fun deleteTask(task: String) {
        _state.update {
            it.copy(tasks = it.tasks - task)
        }

        viewModelScope.launch {
            _effect.emit(TaskEffect.ShowToast("Task Deleted"))
        }
    }
}
```

