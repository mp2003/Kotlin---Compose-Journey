#### **STEP 1 — DEFINE STATE**

👉 Ask:  “What does UI show?”
- Answer:
	- List of tasks
	- Loadin

``` kotlin
data class TaskState(
    val tasks: List<String> = emptyList(),
    val isLoading: Boolean = false
)
```

###### **Remember**
- Holds UI **data**
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


#### TaskScreen (Compose UI)

---

##### What this does
- Observes State from ViewModel
- Sends Intent to ViewModel
- Collects Effect (toast)

---

## Compose UI

```kotlin
package com.milind.composeplayground

import android.widget.Toast
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp
import androidx.lifecycle.viewmodel.compose.viewModel

@Composable
fun TaskScreen(
    viewModel: TaskViewModel = viewModel()
) {

    // 1. Collect State
    val state by viewModel.state.collectAsState()

    // 2. Context for Toast
    val context = LocalContext.current

    // 3. Collect Effect (ONE TIME)
    LaunchedEffect(Unit) {
        viewModel.effect.collect { effect ->
            when (effect) {
                is TaskEffect.ShowToast -> {
                    Toast.makeText(context, effect.message, Toast.LENGTH_SHORT).show()
                }
            }
        }
    }

    // 4. UI Layout
    Column(modifier = Modifier.padding(16.dp)) {

        var text by remember { mutableStateOf("") }

        // Input + Add Button
        Row {
            TextField(
                value = text,
                onValueChange = { text = it },
                modifier = Modifier.weight(1f)
            )

            Spacer(modifier = Modifier.width(8.dp))

            Button(onClick = {
                if (text.isNotEmpty()) {
                    viewModel.onIntent(TaskIntent.AddTask(text))
                    text = ""
                }
            }) {
                Text("Add")
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        // Task List
        LazyColumn {
            items(state.task) { item ->

                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Text(text = item)

                    Button(onClick = {
                        viewModel.onIntent(TaskIntent.DeleteTask(item))
                    }) {
                        Text("Delete")
                    }
                }

                Spacer(modifier = Modifier.height(8.dp))
            }
        }
    }
}
