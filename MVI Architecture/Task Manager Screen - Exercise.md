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

#### **STEP 3 — DEFINE EFFECT**

👉 Ask:  
“What happens once?”
- Answer:
	- Show toast

```kotlin
sealed class TaskEffect {
    data class ShowToast(val message: String) : TaskEffect()
}
```

#### **STEP 4 — CREATE VIEWMODEL (BASIC STRUCTURE)**

```kotlin 
class TaskViewModel : ViewModel() {
    private val _state = MutableStateFlow(TaskState())
    val state = _state

    private val _effect = MutableSharedFlow<TaskEffect>()
    val effect = _effect
}
```

