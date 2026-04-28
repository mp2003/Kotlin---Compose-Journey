###### Definition
- Dependency Injection (DI) = providing dependencies from outside instead of creating them inside a class

###### **What DI actually does**
-  DI **removes responsibility from your class**.
	Without DI, your class is doing **two jobs**:
	1. Business logic
	2. Creating its dependencies
	
	With DI, your class does **only one job**:
	- Business logic
	
	Everything else (object creation, wiring) is handled outside.

###### Example 

-  Without DI : 
  ``` kotlin
  class TaskViewModel : ViewModel() {
    private val repo = TaskRepository()
}
  ```

	What’s happening here:
		- ViewModel decides _which_ repo to use
		- ViewModel decides _how_ to create it
		- ViewModel is tightly coupled to that implementation

- 
  
  
###### What to Look For in Code
- Classes do not create objects using `ClassName()`
- Dependencies are passed in constructor
- `@Inject` used on constructors
- Interfaces used instead of concrete classes
- Hilt modules define bindings
- Entry points annotated with `@AndroidEntryPoint`

###### Why This Matters (Practical)
- You can replace implementations easily
- You can test using fake/mock dependencies
- Object creation is centralized
- No duplicate object creation across app

###### Core Pattern (Always Follow)
- UI → ViewModel → Repository → Data source
- Each layer receives dependency, does not create it

###### How to Convert Existing Code

Step 1 — Identify problem
```kotlin
class TaskViewModel : ViewModel() {
    private val repo = TaskRepository()
}
````

Step 2 — Move dependency to constructor

```kotlin
class TaskViewModel(
    private val repo: TaskRepository
) : ViewModel()
```

Step 3 — Let Hilt provide it

```kotlin
class TaskViewModel @Inject constructor(
    private val repo: TaskRepository
) : ViewModel()
```

###### **Repository Setup (Real Use)**

```kotlin
interface TaskRepository {
    suspend fun getTasks(): List<Task>
}

class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService
) : TaskRepository
```

###### **Binding Interface to Implementation**

```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds
    abstract fun bindTaskRepository(
        impl: TaskRepositoryImpl
    ): TaskRepository
}
```

###### **ViewModel Usage**

```kotlin
@HiltViewModel
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel()
```

###### **Entry Point (Required)**

```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity()
```

###### **Compose Usage**

```kotlin
@Composable
fun TaskScreen(
    viewModel: TaskViewModel = hiltViewModel()
)
```

###### **Full Flow (Read This Twice)**

- UI requests ViewModel
- Hilt creates ViewModel
- ViewModel needs Repository
- Hilt provides Repository
- Repository needs ApiService
- Hilt provides ApiService
- Everything is connected through constructor injection

###### **What to Avoid**

- Creating objects manually inside classes
- Using concrete classes directly in ViewModel
- Mixing DI and manual creation
- Forgetting to bind interfaces

###### **Quick Mental Check**

- “Am I creating this dependency manually?”
- If yes → move it to constructor and inject it
