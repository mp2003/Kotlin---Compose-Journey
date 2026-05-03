###### Elevator Pitch
- The complete copy-paste-ready code for the Task Manager exercise — ViewModel and Compose UI in one place.

---

###### Definition
- The finished implementation of the screen built step-by-step in [[Task Manager Screen - Exercise]]

---

###### Real-World Analogy
- Like the photo on the IKEA box
- Use it as a reference after you have built each piece
- Never start by reading this — build first, check here second

---

###### What
- Full ViewModel code with State, Intent, Effect
- Full Compose UI that observes State, sends Intent, collects Effect

---

###### Why
- One place to compare your work against
- Useful for debugging when something does not match
- Single source for the finished screen

---

###### Full ViewModel

```kotlin
// File: TaskViewModel.kt

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.MutableSharedFlow
import kotlinx.coroutines.flow.update
import kotlinx.coroutines.launch

// ---- STATE ----
// Holds everything the screen displays
data class TaskState(
    val tasks: List<String> = emptyList(),   // visible list
    val isLoading: Boolean = false            // spinner flag
)

// ---- INTENT ----
// Closed set of user actions
sealed class TaskIntent {
    data class AddTask(val task: String) : TaskIntent()
    data class DeleteTask(val task: String) : TaskIntent()
}

// ---- EFFECT ----
// One-time UI events
sealed class TaskEffect {
    data class ShowToast(val message: String) : TaskEffect()
}

// ---- VIEWMODEL ----
class TaskViewModel : ViewModel() {

    // State stream (private writer, public reader)
    private val _state = MutableStateFlow(TaskState())
    val state = _state

    // Effect stream (private emitter, public collector)
    private val _effect = MutableSharedFlow<TaskEffect>()
    val effect = _effect

    // Single entry point for every user action
    fun onIntent(intent: TaskIntent) {
        when (intent) {
            is TaskIntent.AddTask    -> addTask(intent.task)
            is TaskIntent.DeleteTask -> deleteTask(intent.task)
        }
    }

    // ---- INTERNAL LOGIC ----

    // State update only
    private fun addTask(task: String) {
        _state.update {
            it.copy(tasks = it.tasks + task)
        }
    }

    // State update + one-time effect
    private fun deleteTask(task: String) {
        _state.update {
            it.copy(tasks = it.tasks - task)
        }

        // launch a coroutine to emit the effect
        viewModelScope.launch {
            _effect.emit(TaskEffect.ShowToast("Task Deleted"))
        }
    }
}
```

---

###### Full Compose UI

```kotlin
// File: TaskScreen.kt

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

    // 1. Read State stream as a Compose state
    val state by viewModel.state.collectAsState()

    // 2. Context needed for Toast
    val context = LocalContext.current

    // 3. Collect one-time effects
    LaunchedEffect(Unit) {
        viewModel.effect.collect { effect ->
            when (effect) {
                is TaskEffect.ShowToast -> {
                    Toast.makeText(context, effect.message, Toast.LENGTH_SHORT).show()
                }
            }
        }
    }

    // 4. UI layout
    Column(modifier = Modifier.padding(16.dp)) {

        // Local input text held in Compose state
        var text by remember { mutableStateOf("") }

        // ---- Input row ----
        Row {
            TextField(
                value = text,
                onValueChange = { text = it },
                modifier = Modifier.weight(1f)
            )

            Spacer(modifier = Modifier.width(8.dp))

            Button(onClick = {
                if (text.isNotEmpty()) {
                    // send AddTask Intent
                    viewModel.onIntent(TaskIntent.AddTask(text))
                    text = ""   // clear input after add
                }
            }) {
                Text("Add")
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        // ---- Task list ----
        LazyColumn {
            items(state.tasks) { item ->

                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Text(text = item)

                    Button(onClick = {
                        // send DeleteTask Intent
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
```

---

###### Memory Hook
- "Build first, peek here second."

---

###### Key Rule
- This file is a reference, not a tutorial — go to [[Task Manager Screen - Exercise]] to learn

---

###### Related
- [[Task Manager Screen - Exercise]]
