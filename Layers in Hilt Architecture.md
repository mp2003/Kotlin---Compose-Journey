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

#### **2. Domain Layer**
###### **What it contains**

- Interfaces (contracts)
- Business rules (optional)

###### **Example**
```kotlin 
interface TaskRepository {
    fun getTasks(): List<String>
}
```

###### **Responsibility**

- Define **what needs to be done**
- No Android, no Retrofit, no DB

###### **Key Points**

- Pure Kotlin
- No dependencies
- Most stable layer

---

#### **3. Data Layer**

###### **What it contains**

- Implementations of domain interfaces
- API / DB logic

###### Example

```kotlin
class TaskRepositoryImpl @Inject constructor() : TaskRepository {

    override fun getTasks(): List<String> {
        return listOf("Task 1", "Task 2")
    }
}
```

###### **Responsibility**

- Decide **how data is fetched**
- Connect to network / database

###### **Key Points**

- Depends on Domain
- Can change freely without affecting UI

---

## **4. DI Layer (Hilt Layer)**

###### **What it contains**

- Modules (`@Module`)
- Bindings (`@Binds`, `@Provides`)

###### Example 

```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds
    abstract fun bindRepo(
        impl: TaskRepositoryImpl
    ): TaskRepository
}
```

### **Responsibility**

- Tell Hilt:
    - Which implementation to use
    - How to create objects

### **Key Points**

- Connects everything together
- No business logic
  
  ---


Dependency Direction (Very Important)