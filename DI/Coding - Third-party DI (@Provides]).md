  Step 3 — Third-party DI  - @Provides

## **Why this step exists**

You CANNOT do this:

```kotlin
class Retrofit @Inject constructor() // ❌ not your class
```

So:

- `@Inject` ❌
- `@Binds` ❌

👉 You use **`@Provides`**

---

# **🎯 Goal**

```text
ViewModel
   ↓
Repository
   ↓
ApiService
   ↓
Retrofit
```

---

# **Step 1 — Create ApiService**

### 

### **📍**

**`data/remote/ApiService.kt`**

```kotlin
interface ApiService {
    fun getTasks(): List<String>
}
```

Fake for now:

```kotlin
class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> {
        return listOf("API Task 1", "API Task 2")
    }
}
```

---

# **Step 2 — Update Repository**

### 

### **📍**

**`TaskRepositoryImpl.kt`**

```kotlin
class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService
) : TaskRepository {

    override fun getTasks(): List<String> {
        return api.getTasks()
    }
}
```

---

# **Step 3 — Provide ApiService (IMPORTANT)**

### 

### **📍**

**`di/NetworkModule.kt`**

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

# **🔁 What just happened**

- Hilt sees ViewModel needs `TaskRepository`
- Repository needs `ApiService`
- Hilt checks:
    - “Who provides ApiService?” → NetworkModule
- Uses `@Provides` → creates instance

---

# **🧠 Key Understanding**

### **Use this rule always:**

|**Situation**|**Use**|
|---|---|
|You own class|`@Inject constructor`|
|Interface binding|`@Binds`|
|External / manual creation|`@Provides`|

---

# **Step 4 — Run flow**

```text
Compose → ViewModel
         → Repository
         → ApiService
         → FakeApiService
```

---

# **🧪 Your Task**

1. Create `ApiService`
2. Inject into `Repository`
3. Create `NetworkModule`
4. Run app

---

# **❌ Common errors**

- Forgot `@Module`
- Forgot `@InstallIn`
- Missing import for `@Provides`
- Still using old constructor without ApiService

---

# **✅ When done**

Update roadmap:

```text
### Third-party
- [x] Provide Retrofit / OkHttp (FakeApi for now)
- [ ] Use qualifiers
```

---

# **👉 Next step after this**

- Qualifiers (`@Named`, multiple APIs)
- Then real Retrofit

---

Reply:

- “working”  
    or paste error

I’ll fix instantly.