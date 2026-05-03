##### **1. Presentation Layer**

###### **What it contains**

- Compose UI (Screens)
- ViewModel

###### **Responsibility**

- Handle UI + state
- Call business logic
- Do NOT know how data is fetched

---

###### **Example**

```kotlin
@HiltViewModel
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel()
```

###### **Key Points**

- Depends on **Domain layer**
- Uses **interfaces only**
- Never uses implementation (`TaskRepositoryImpl`) directly

---

