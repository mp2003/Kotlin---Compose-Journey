---
up: "[[00 - DI - Overview]]"
---

###### Elevator Pitch
- `@Binds` and `@Provides` both teach Hilt how to supply a dependency, but `@Binds` is for "I already have a constructor-injected class, just hand it over as the interface" and `@Provides` is for "I have to build this object myself".

---

###### Definition
- Two annotations inside a Hilt `@Module` that tell the compiler how to satisfy a dependency request — one by metadata, one by running real code

---

###### Real-World Analogy
- @Binds -> a name-tag swap at a meeting: "Bob, you are now 'The Speaker'" — no work, just a label
- @Provides -> a chef: actually cooks the dish before handing it over

---

###### What
- Both live inside a `@Module` annotated with `@InstallIn(...)`
- Both teach Hilt: "when someone asks for type X, here is how to give it"
- Difference is HOW the object is produced

---

###### Why
- Different use cases need different mechanisms
- @Binds is faster (compile-time only, no runtime function call)
- @Provides is more powerful (you can run any code)
- Picking the right one keeps modules clean and small

---

###### Core Concepts
- @Binds -> abstract function, no body, just metadata
- @Provides -> normal function, real body, returns an object
- @Binds requires `abstract class` module
- @Provides usually goes in an `object` module
- Link: [[03 - Hilt - DI Library]], [[07 - Hilt - Third-party with @Provides]]

---

###### Side-by-Side Comparison

```text
+---------------------+--------------------------------+--------------------------------+
| Aspect              | @Binds                         | @Provides                      |
+---------------------+--------------------------------+--------------------------------+
| Module type         | abstract class                 | object (or class)              |
| Function body       | none (abstract)                | real code                      |
| Use when            | impl has @Inject constructor   | you must build the object      |
| Cost                | compile-time only              | runtime function call          |
| Typical job         | interface -> impl              | Retrofit / OkHttp / Room       |
| Can take params     | exactly one (the impl)         | any number (other deps)        |
+---------------------+--------------------------------+--------------------------------+
```

---

###### When to Use @Binds
- You have an interface (`TaskRepository`)
- You have an implementation (`TaskRepositoryImpl`) that already has `@Inject constructor`
- You just need to tell Hilt "interface -> this impl"
- Nothing to build, nothing to configure

---

###### When to Use @Provides
- The object cannot be built with `@Inject` (third-party class, no constructor access)
- You need configuration (base URL, timeouts, builders)
- You need conditional logic (different impl in debug vs release)
- You need to combine multiple dependencies into one object

---

###### Example (Step-by-step)

**Step 1 — @Binds: interface to impl**

```kotlin
// abstract class -> required for @Binds
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    // Hilt reads this as metadata only
    // Translates to: "if asked for TaskRepository, return TaskRepositoryImpl"
    @Binds
    abstract fun bindTaskRepository(
        impl: TaskRepositoryImpl   // must be @Inject-constructed
    ): TaskRepository
}
```

- Pure wiring, zero runtime cost

**Step 2 — @Provides: build the object yourself**

```kotlin
// object -> simplest container for @Provides
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    // Real function body -> Hilt CALLS this when an OkHttpClient is needed
    @Provides
    @Singleton
    fun provideOkHttp(): OkHttpClient =
        OkHttpClient.Builder()
            .connectTimeout(30, TimeUnit.SECONDS)
            .build()

    // Can take other deps as params -> Hilt fills them in
    @Provides
    @Singleton
    fun provideRetrofit(client: OkHttpClient): Retrofit =
        Retrofit.Builder()
            .baseUrl("https://api.example.com/")
            .client(client)
            .build()

    // Retrofit interfaces are generated -> can't @Inject them, must @Provides
    @Provides
    fun provideApiService(retrofit: Retrofit): ApiService =
        retrofit.create(ApiService::class.java)
}
```

- Each function does real work, can pull in other dependencies

---

###### Full Example

```kotlin
// =========================================================
// 1. Repository: @Binds wires interface -> impl
// =========================================================
interface TaskRepository {
    fun getTasks(): List<String>
}

class TaskRepositoryImpl @Inject constructor(
    private val api: ApiService
) : TaskRepository {
    override fun getTasks() = api.getTasks()
}

@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {
    @Binds
    abstract fun bindRepo(impl: TaskRepositoryImpl): TaskRepository
}

// =========================================================
// 2. Network: @Provides builds Retrofit + ApiService
// =========================================================
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides @Singleton
    fun provideOkHttp(): OkHttpClient =
        OkHttpClient.Builder().build()

    @Provides @Singleton
    fun provideRetrofit(client: OkHttpClient): Retrofit =
        Retrofit.Builder()
            .baseUrl("https://api.example.com/")
            .client(client)
            .build()

    @Provides
    fun provideApiService(retrofit: Retrofit): ApiService =
        retrofit.create(ApiService::class.java)
}
```

---

###### Decision Tree

```text
Need to satisfy a dependency?
        |
        v
Does the impl have @Inject constructor + you just want interface -> impl?
        |
   YES  |   NO
        v   v
   @Binds   @Provides
```

---

###### Common Mistakes
- BAD: using @Binds for a class without `@Inject constructor` -> compile error
- BAD: putting @Provides inside an `abstract class` (works, but mixes concerns)
- BAD: using @Provides where @Binds works -> slower and more boilerplate
- BAD: forgetting `@InstallIn` on the module -> binding never registers

---

###### Weak Area Clarification
- Confusion: "Why two ways to do the same thing?"
- Why: they look similar but solve different problems
- Resolution: @Binds = swap label, @Provides = build the object. If the class has `@Inject constructor`, prefer @Binds; otherwise @Provides

---

###### Common Follow-up Traps
- Q: Can I mix @Binds and @Provides in one module?
  A: Only if the module is an `abstract class` AND the @Provides function is `companion object` — easier to keep them in separate modules
- Q: Why is @Binds faster?
  A: It generates direct factory code at compile time, no runtime function call
- Q: Can @Binds take more than one parameter?
  A: No — exactly one parameter, the implementation it binds to

---

###### Memory Hook
- Binds = label swap, Provides = build the thing

---

###### Key Rule
- If `@Inject constructor` exists -> @Binds. If you must build it -> @Provides.

---

###### Related
- [[00 - DI - Overview]]
- [[03 - Hilt - DI Library]]
- [[07 - Hilt - Third-party with @Provides]]
