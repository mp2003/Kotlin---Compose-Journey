---
up: "[[00 - DI - Overview]]"
---

###### Elevator Pitch
- Dependency Inversion means high-level code depends on abstractions (interfaces), not concrete implementations, so systems stay flexible and testable.

---

###### Definition
- Dependency Inversion Principle (DIP) states that high-level modules should not depend on low-level modules, both should depend on abstractions.

---

###### Real-World Analogy
- Switch and bulb:
  - Switch (high-level) does NOT care about bulb type
  - Bulb (LED, CFL) can change anytime
- Interface = socket, implementation = bulb

---

###### What
- High-level = ViewModel (business logic)
- Low-level = RepositoryImpl, ApiService
- Abstraction = interface (TaskRepository)
- Direction of dependency is reversed using interface

---

###### Why
- Makes code replaceable (swap FakeApi → RealApi)
- Enables easy testing (use fake repository)
- Prevents tight coupling between layers

---

###### Core Concepts
- High-level module -> main logic (ViewModel)
- Low-level module -> implementation (RepositoryImpl)
- Abstraction -> interface contract (TaskRepository)
- Dependency direction -> both depend on abstraction
- Link: [[Hilt - DI]], [[Hilt - Qualifiers]]

---

###### How it Works
- Create interface in domain layer
- ViewModel depends on interface
- Implementation lives in data layer
- DI (Hilt) connects interface → implementation

---

###### ASCII Flowchart

```text
ViewModel (High-level)
        ↓
   TaskRepository (Interface)
        ↑
TaskRepositoryImpl (Low-level)
        ↓
   ApiService
````

---

###### **Layered Diagram**

```text
+----------------------+
| Presentation         |
| ViewModel            |
+----------------------+
| Domain               |
| TaskRepository       |
+----------------------+
| Data                 |
| TaskRepositoryImpl   |
| ApiService           |
+----------------------+
```

---

###### **Example (Step-by-step)**

**Step 1 — Define abstraction**

```kotlin
// Interface = contract (no implementation)
interface TaskRepository {
    fun getTasks(): List<String> // defines behavior
}
```

- ViewModel will depend on this, not implementation

---

**Step 2 — Implement it**

```kotlin
// Concrete implementation (low-level)
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService // dependency
) : TaskRepository {

    override fun getTasks(): List<String> {
        return api.getTasks() // actual logic
    }
}
```

- Implementation depends on lower layer (API)

---

**Step 3 — Use in ViewModel**

```kotlin
// High-level module
@HiltViewModel
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository // depends on abstraction
)
```

- ViewModel does NOT know about TaskRepositoryImpl

---

###### **Full Example**

```kotlin
// =========================================================
// Full DIP flow with DI
// =========================================================

// Domain (abstraction)
interface TaskRepository {
    fun getTasks(): List<String>
}

// Data (implementation)
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService
) : TaskRepository {

    override fun getTasks(): List<String> {
        return api.getTasks()
    }
}

// DI (binding)
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds
    abstract fun bindRepo(
        impl: TaskRepositoryImpl
    ): TaskRepository
}

// Presentation (high-level)
@HiltViewModel
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository
)
```

---

###### **Common Mistakes**

- BAD: ViewModel using TaskRepositoryImpl directly
- BAD: Skipping interface layer
- BAD: Putting business logic in data layer
- BAD: Tight coupling between layers

---

###### **Weak Area Clarification**

- Confusion: “Why not directly use implementation?”
- Problem: hard to change, test, or scale
- Fix: depend on interface, inject implementation

---

###### **Common Follow-up Traps**

- Q: Is DIP same as DI?  
    A: No, DIP is principle, DI is implementation
- Q: Can DI exist without DIP?  
    A: Yes, but design will be poor
- Q: Why interface needed?  
    A: To decouple high-level from low-level

---

###### **Memory Hook**

- Depend on WHAT, not HOW

---

###### **Key Rule**

- High-level code must never depend on concrete implementation

---

###### Keywords Used

| Keyword | What it does |
|---------|--------------|
| `interface TaskRepository` | Defines the abstraction (contract) that high-level code depends on |
| `fun getTasks(): List<String>` | Declares behavior without providing implementation |
| `@Inject constructor(private val api: ApiService)` | Tells Hilt to inject an ApiService when building this class |
| `override fun getTasks()` | Provides the concrete implementation of the interface method |
| `@HiltViewModel` | Marks the ViewModel so Hilt knows it must build it |
| `@Inject constructor(private val repository: TaskRepository)` | Injects the interface type — Hilt resolves which impl to use |
| `@Module` | Container of Hilt binding rules |
| `@InstallIn(SingletonComponent::class)` | Installs the module's rules into the app-wide component |
| `@Binds` | Metadata rule: "when asked for TaskRepository, give TaskRepositoryImpl" |
| `abstract fun bindRepo(impl: TaskRepositoryImpl): TaskRepository` | Wires the interface to the implementation at compile time |

---

###### **Related**

- [[Hilt - DI]]
- [[Hilt - @Provides vs @Binds]]

````
---

# 🧠 Final clarity (outside notes)

👉 What you’ve been doing:

```text
ViewModel → TaskRepository
````

👉 NOT:

```text
ViewModel → TaskRepositoryImpl
```

That **is DIP in action**.

---

# **🔥 Final one-line understanding**

```text
DI = tool
DIP = design rule
```

---

# **🚀 You are done with WEEK 2 core**

If you want next:

👉 WEEK 3 (Networking + Retrofit + Real API)  
👉 Or deeper: Testing with Hilt (this is next real skill)