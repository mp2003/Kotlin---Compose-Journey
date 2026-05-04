---
up: "[[Hilt - Overview]]"
---
###### Elevator Pitch
- Qualifiers tell Hilt exactly which dependency to inject when multiple implementations of the same type exist.

---

###### Definition
- A qualifier is an annotation used to uniquely identify a specific dependency among multiple bindings of the same type.

---

###### Real-World Analogy
- Restaurant:
  - You order "coffee" -> waiter asks "which one?"
  - You say "cold coffee" -> specific choice
- Qualifier = your specific order label

---

###### What
- Used when multiple implementations of same type exist
- Acts as a tag or label on dependencies
- Applied at provider and injection point
- Prevents ambiguity in DI graph

---

###### Why
- Avoids "multiple bindings" error
- Ensures correct dependency is injected
- Allows switching implementations easily

---

###### Core Concepts
- @Qualifier -> custom annotation to label dependency
- @Named -> built-in string-based qualifier
- Provider -> place where object is created (@Provides)
- Injection point -> where object is used (constructor)
- Link: [[Hilt - DI]], [[Hilt - @Provides vs @Binds]]

---

###### How it Works
- Define qualifier annotation
- Attach qualifier to provider method
- Attach same qualifier at injection point
- Hilt matches qualifier and injects correct instance

---

###### ASCII Flowchart

```text
Multiple ApiService
        |
        v
Hilt sees ambiguity
        |
        v
Check qualifier (@FakeApi)
        |
        v
Match provider
        |
        v
Inject correct dependency
````

---

###### **Layered Diagram**

```text
+-----------------------+
| ViewModel             |
| needs ApiService      |
+-----------------------+
| Repository            |
| uses ApiService       |
+-----------------------+
| Module                |
| provides Fake/Real    |
+-----------------------+
```

---

###### **Example (Step-by-step)**

**Step 1 — Create Qualifier**

```kotlin
// @Qualifier marks this as a dependency label
@Qualifier
@Retention(AnnotationRetention.BINARY) // keeps it at compile time
annotation class FakeApi
```

- This defines a label called FakeApi

---

**Step 2 — Provide dependency**

```kotlin
// @Provides tells Hilt how to create the object
@FakeApi // attach qualifier
@Provides
fun provideFakeApi(): ApiService {
    return FakeApiService() // actual implementation
}
```

- This tells Hilt: FakeApi = FakeApiService

---

**Step 3 — Inject dependency**

```kotlin
// constructor injection
class Repo @Inject constructor(
    @FakeApi private val api: ApiService // request specific one
)
```

- This tells Hilt which ApiService to use

---

###### **Full Example**

```kotlin
// =========================================================
// Full DI flow with qualifier
// =========================================================

// Step 1: Qualifier
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class FakeApi

// Step 2: Interface
interface ApiService {
    fun getTasks(): List<String>
}

// Step 3: Implementation
class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> {
        return listOf("Fake Task")
    }
}

// Step 4: Module
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @FakeApi
    @Provides
    fun provideFakeApi(): ApiService {
        return FakeApiService()
    }
}

// Step 5: Usage
class Repo @Inject constructor(
    @FakeApi private val api: ApiService
)
```

---

###### **Common Mistakes**

- BAD: Using qualifier only in provider, not in constructor
- BAD: Mixing @Named and custom qualifier incorrectly
- BAD: Applying @Qualifier on class instead of annotation
- BAD: Forgetting qualifier when multiple implementations exist

---

###### **Weak Area Clarification**

- Confusion: “Why not auto-inject?”
- Reason: Multiple implementations → ambiguity
- Fix: Use qualifier to remove ambiguity

---

###### **Common Follow-up Traps**

- Q: Can Hilt decide automatically?  
    A: No, if multiple bindings exist
- Q: Is @Named enough?  
    A: Yes, but less type-safe
- Q: When to prefer custom qualifier?  
    A: Always in production code

---

###### **Memory Hook**

- Same type + multiple options = need a label

---

###### **Key Rule**

- If multiple implementations exist, always use a qualifier

---

###### **Related**

- [[Hilt - DI]]
- [[Hilt - @Provides vs @Binds]]