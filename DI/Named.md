---
up: "[[DI - Overview]]"
---
###### Elevator Pitch
- Custom Qualifiers and @Named both solve the same problem (choosing between multiple dependencies), but custom qualifiers are type-safe while @Named relies on strings.

---

###### Definition
- Qualifiers are annotations used to distinguish dependencies, while @Named is a built-in qualifier that uses string keys.

---

###### Real-World Analogy
- Restaurant:
  - Saying "coffee: cold" (string) -> @Named
  - Saying "ColdCoffeeTag" (specific label) -> custom qualifier
- Custom label = safer, string = error-prone

---

###### What
- Both are used when multiple implementations exist
- @Named uses strings
- Custom qualifiers use annotation classes
- Both must be used in provider and injection point

---

###### Why
- Avoid ambiguity in DI graph
- Choose correct implementation
- Improve readability and maintainability

---

###### Core Concepts
- @Named("key") -> string-based identifier
- @Qualifier -> custom annotation identifier
- Matching -> same identifier must be used in provider and injection
- Type safety -> compile-time vs runtime safety
- Link: [[Hilt - Qualifiers]], [[Hilt - DI]]

---

###### How it Works
- Multiple implementations exist
- Add identifier (@Named or custom)
- Apply identifier in provider
- Apply same identifier in constructor
- Hilt matches and injects correct dependency

---

###### ASCII Flowchart

```text
Multiple Implementations
        |
        v
Need to choose one
        |
        v
Use identifier
   /            \
@Named        @Qualifier
(string)      (type-safe)
        |
        v
Hilt matches and injects
````

---

###### **Comparison**

- @Named:
    - Uses string (“fake”, “real”)
    - Quick to write
    - Not type-safe
    - Typos cause runtime issues
- Custom Qualifier:
    - Uses annotation class (@FakeApi)
    - More readable
    - Compile-time safe
    - Preferred in production

---

###### **Example (Step-by-step)**

**Step 1 — Using @Named**

```kotlin
// Provide dependency with string label
@Provides
@Named("fake")
fun provideFakeApi(): ApiService = FakeApiService()
```

```kotlin
// Inject using same string
class Repo @Inject constructor(
    @Named("fake") private val api: ApiService
)
```

- Uses string to match dependency

---

**Step 2 — Using Custom Qualifier**

```kotlin
// Define qualifier annotation
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class FakeApi
```

```kotlin
// Provide dependency with qualifier
@Provides
@FakeApi
fun provideFakeApi(): ApiService = FakeApiService()
```

```kotlin
// Inject using qualifier
class Repo @Inject constructor(
    @FakeApi private val api: ApiService
)
```

- Uses type-safe annotation

---

###### **Full Example**

```kotlin
// =========================================================
// @Named vs Custom Qualifier
// =========================================================

// Interface
interface ApiService {
    fun getTasks(): List<String>
}

// Implementation
class FakeApiService @Inject constructor() : ApiService {
    override fun getTasks(): List<String> {
        return listOf("Fake Task")
    }
}

// Using @Named
@Module
@InstallIn(SingletonComponent::class)
object NamedModule {

    @Provides
    @Named("fake")
    fun provideFakeApi(): ApiService = FakeApiService()
}

class NamedRepo @Inject constructor(
    @Named("fake") private val api: ApiService
)

// Using Custom Qualifier
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class FakeApi

@Module
@InstallIn(SingletonComponent::class)
object QualifierModule {

    @Provides
    @FakeApi
    fun provideFakeApi(): ApiService = FakeApiService()
}

class QualifierRepo @Inject constructor(
    @FakeApi private val api: ApiService
)
```

---

###### **Common Mistakes**

- BAD: Using different string values in @Named
- BAD: Typos in @Named (“Fake” vs “fake”)
- BAD: Mixing @Named and custom qualifiers inconsistently
- BAD: Not using qualifier when multiple implementations exist

---

###### **Weak Area Clarification**

- Confusion: “Both do same thing, why two?”
- Reason: @Named is simple, custom is safe
- Rule: Use @Named for quick work, custom for production

---

###### **Common Follow-up Traps**

- Q: Which one is better?  
    A: Custom qualifier (type-safe)
- Q: When to use @Named?  
    A: Small/simple cases
- Q: Can both be used together?  
    A: Yes, but avoid mixing unnecessarily

---

###### **Memory Hook**

- @Named = string, @Qualifier = strong type

---

###### **Key Rule**

- Prefer custom qualifiers over @Named for safety and clarity

---

###### **Related**

- [[Hilt - Qualifiers]]
- [[Hilt - DI]]