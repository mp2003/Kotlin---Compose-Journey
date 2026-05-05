---
up: "[[00 - DI - Overview]]"
---
###### Definition
- Injection is the process of providing dependencies to a class instead of creating them inside the class

###### What
- Dependencies are passed via constructor
- Hilt creates and provides required objects
- Class only declares what it needs
- No manual object creation inside class

###### Why
- Removes tight coupling
- Makes code testable
- Allows changing implementation easily
- Keeps classes focused on logic only

###### Core Concepts
- Constructor Injection → primary way (`@Inject constructor`)
- Dependency → required object (e.g., Repository)
- Consumer → class using dependency (e.g., ViewModel)
- Hilt → provides dependencies
- Interface → used instead of concrete class

###### How it Works
- Class declares dependency in constructor
- Constructor marked with `@Inject`
- Hilt builds dependency graph
- Hilt creates and injects dependency at runtime

###### Flow
- UI → ViewModel
- ViewModel → Repository (injected)
- Repository → Data source (injected)
- Hilt connects everything

###### Usage (Pattern)
- Do not create objects manually
- Always pass dependencies via constructor
- Use interfaces for abstraction
- Let Hilt manage creation

###### Example (Before Injection)
```kotlin
class TaskViewModel : ViewModel() {
    private val repository = TaskRepositoryImpl()
}
```

**After**

``` kotlin
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel()
```

###### **Binding (Important)**

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

###### **What to Look For in Code**

- `@Inject constructor`
- No `ClassName()` inside classes
- Interface used in ViewModel
- Module binds implementation
- Hilt provides dependency automatically

###### **Common Mistakes**

- Creating dependencies manually
- Using concrete class instead of interface
- Forgetting to bind interface
- Mixing manual creation with DI

###### **Key Rule**

- Class should declare dependencies, not create them


