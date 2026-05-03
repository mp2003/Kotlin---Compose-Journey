###### Definition
- `@Provides` tells Hilt how to create a dependency you do not own (e.g. Retrofit, OkHttp)

---

###### What
- Used for classes you cannot annotate with `@Inject` (third-party / external libraries)
- Lives inside a `@Module` annotated with `@InstallIn`
- A function returns the instance Hilt should inject
- Replaces `@Inject` and `@Binds` for external types

---

###### Why
- You cannot edit a third-party class to add `@Inject constructor`
- `@Binds` only works for interface → implementation mapping you own
- `@Provides` lets you write the creation logic manually
- Keeps third-party setup centralized in one module

---

###### Core Concepts
- `@Module` → container for provider functions
- `@InstallIn(SingletonComponent::class)` → scope of the module
- `@Provides` → marks the function that creates the dependency
- Pick the right tool:
    - Own class → `@Inject constructor`
    - Interface binding → `@Binds`
    - External / manual creation → `@Provides`
- Related: [[Hilt]], [[Dependency Injection]], [[Injection]]

---

###### How it Works
- ViewModel asks Hilt for `TaskRepository`
- Repository needs `ApiService`
- Hilt looks up "who provides `ApiService`?"
- Finds `NetworkModule.provideApiService()`
- Calls the `@Provides` function and injects the result

Flow:
```text
Compose → ViewModel → Repository → ApiService → FakeApiService
```

---

###### Example

**Step 1 — Create ApiService** (`data/remote/ApiService.kt`)
```kotlin
interface ApiService {
    fun getTasks(): List<String>
}

class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> {
        return listOf("API Task 1", "API Task 2")
    }
}
```

**Step 2 — Update Repository** (`TaskRepositoryImpl.kt`)
```kotlin
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService
) : TaskRepository {

    override fun getTasks(): List<String> {
        return api.getTasks()
    }
}
```

**Step 3 — Provide ApiService** (`di/NetworkModule.kt`)
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideApiService(): ApiService {
        return FakeApiService()
    }
}
```

---

###### Common Mistakes
- ❌ Forgetting `@Module` on the object
- ❌ Forgetting `@InstallIn(SingletonComponent::class)`
- ❌ Missing import for `@Provides`
- ❌ Repository still using old constructor without `ApiService`
- ❌ Trying to use `@Inject` on a class you don't own

---

###### Key Rule
- If you cannot edit the class, provide it with `@Provides` inside a `@Module`

---

###### Related
- [[Hilt]]
- [[Dependency Injection]]
- [[Injection]]
- [[Layers in Hilt Architecture]]
