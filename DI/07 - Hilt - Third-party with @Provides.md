---
up: "[[03 - Hilt - DI Library]]"
---
 what is 
###### Elevator Pitch
- `@Provides` is how you tell Hilt to build something you cannot edit, like Retrofit or OkHttp, by writing the creation steps yourself inside a Module.

---

###### Definition
- `@Provides` is a Hilt annotation on a function inside a `@Module` that returns a built instance for Hilt to inject

---

###### Real-World Analogy
- Hilt = a kitchen that cooks meals on demand
- Your own classes = ingredients you grow -> Hilt knows the recipe (`@Inject`)
- Third-party class = store-bought sauce -> you hand the kitchen a recipe card
- That recipe card = `@Provides`

---

###### What
- For classes you cannot annotate with `@Inject` (no source access)
- Lives inside a `@Module` marked with `@InstallIn`
- Function body holds the creation steps
- Replaces `@Inject` and `@Binds` for external types

---

###### Why
- You cannot edit Retrofit's source to add `@Inject`
- `@Binds` only maps your own interface to your own class
- `@Provides` lets you write any creation logic
- Keeps third-party setup in one file

---

###### Core Concepts
- `@Module` -> container of recipe functions
- `@InstallIn(SingletonComponent::class)` -> recipes live for the whole app
- `@Provides` -> marks a function as a recipe
- Decision rule:
    - Own class -> `@Inject constructor`
    - Own interface + implementation -> `@Binds`
    - Foreign class -> `@Provides`
- Link: [[03 - Hilt - DI Library]], [[00 - DI - Overview]], [[01 - Injection - Constructor Injection]]

---

###### How it Works
- UI asks Hilt for a ViewModel
- ViewModel needs `TaskRepository`
- Repository needs `ApiService`
- Hilt finds `provideApiService()` in `NetworkModule`
- Hilt runs the function and injects the result

---

###### ASCII Flowchart

```text
[ Compose UI ]
      |
      v
[ ViewModel ]
      |
      v
[ Repository ]
      |
      v
[ ApiService ]
      |
      v
[ @Provides recipe ]
      |
      v
[ FakeApiService ]
```

---

###### Example (Step-by-step)

**Step 1 — ApiService interface** (`data/remote/ApiService.kt`)

```kotlin
// Contract: anyone playing this role must offer getTasks()
interface ApiService {
    fun getTasks(): List<String>            // returns list of task names
}

// Fake worker until real network is ready
// '@Inject constructor()' = Hilt can build this for free
class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> {
        return listOf("API Task 1", "API Task 2")   // hard-coded data
    }
}
```

- Interface = swappable contract
- Fake = stand-in built directly by Hilt

**Step 2 — Repository asks for ApiService** (`TaskRepositoryImpl.kt`)

```kotlin
// '@Inject constructor(api)' = Hilt, please pass me an ApiService
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService             // declared, not created
) : TaskRepository {

    override fun getTasks(): List<String> {
        return api.getTasks()               // delegates to api
    }
}
```

- Repository declares the need, never builds it

**Step 3 — Recipe for ApiService** (`di/NetworkModule.kt`)

```kotlin
// '@Module' = this object holds recipes
// '@InstallIn(...)' = keep recipes alive for whole app
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    // '@Provides' = run this when ApiService is needed
    @Provides
    fun provideApiService(): ApiService {
        return FakeApiService()             // build and return
    }
}
```

- One file owns "how to build ApiService"

---

###### Full Example

```kotlin
// =========================================================
// All three pieces wired together
// =========================================================

// --- Contract --------------------------------------------
interface ApiService {
    fun getTasks(): List<String>            // role to fulfill
}

// --- Fake worker -----------------------------------------
class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> =
        listOf("API Task 1", "API Task 2")  // hard-coded data
}

// --- Repository uses the contract ------------------------
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService             // injected by Hilt
) : TaskRepository {
    override fun getTasks(): List<String> = api.getTasks()
}

// --- Recipe for ApiService -------------------------------
@Module                                     // recipes live here
@InstallIn(SingletonComponent::class)       // app-wide scope
object NetworkModule {

    @Provides                               // mark as recipe
    fun provideApiService(): ApiService {   // Hilt calls this
        return FakeApiService()             // build instance
    }
}
```

---

###### Common Mistakes
- BAD: Forgetting `@Module` -> Hilt ignores the object
- BAD: Forgetting `@InstallIn` -> build fails
- BAD: Missing `@Provides` import -> function invisible to Hilt
- BAD: Repository keeps old empty constructor
- BAD: Adding `@Inject` to a class you do not own

---

###### Common Follow-up Traps
- Q: Why not `FakeApiService()` directly inside Repository?
  A: Couples it to one impl, breaks testing, defeats DI.
- Q: When use `@Binds` instead of `@Provides`?
  A: When you own both interface and impl with no creation logic.
- Q: Why is `NetworkModule` an `object`?
  A: No state needed, singleton avoids extra instantiation.
- Q: Can `@Provides` depend on other things?
  A: Yes — list them as parameters and Hilt injects them.
- Q: What if I forget `@InstallIn`?
  A: Build fails — Hilt requires every Module to declare scope.

---

###### Memory Hook
- "Don't own it? Provide it."

---

###### Key Rule
- Three doors: own class -> `@Inject`, own interface -> `@Binds`, foreign class -> `@Provides`

---

###### Related
- [[03 - Hilt - DI Library]]
