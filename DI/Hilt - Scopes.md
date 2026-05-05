---
up: "[[DI - Overview]]"
---

###### Elevator Pitch
- A Hilt scope decides how long a single instance of a dependency lives — for the whole app, for one Activity, for one ViewModel, or for every fresh request.

---

###### Definition
- An annotation that tells Hilt to reuse the same instance within a specific component's lifetime instead of creating a new one each time

---

###### Real-World Analogy
- @Singleton -> one CEO for the entire company
- @ActivityScoped -> one manager per branch office
- @ViewModelScoped -> one assistant assigned to one task only
- No scope -> a fresh intern every time someone asks

---

###### What
- A scope ties an object's lifetime to a Hilt component
- Same scope + same component = same instance
- Different component = different instance
- No scope = new instance every injection

---

###### Why
- Avoid creating expensive objects multiple times (Retrofit, Database)
- Share state between collaborators within a screen
- Prevent memory leaks by tying lifetime to the right component
- Match the natural lifecycle of Android (Activity, Fragment, ViewModel)

---

###### Core Concepts
- Component -> a Hilt-managed container with a lifetime
- Scope -> annotation that says "live as long as THIS component"
- @InstallIn(X::class) -> "install this module into component X"
- Scope on @Provides/@Binds -> "cache the result inside that component"
- Link: [[Hilt - DI Library]], [[Hilt - Architecture Layers]]

---

###### Scope Cheat Sheet

```text
+------------------------+----------------------------+----------------------------+
| Scope                  | Lives as long as           | Component                  |
+------------------------+----------------------------+----------------------------+
| @Singleton             | the Application            | SingletonComponent         |
| @ActivityRetainedScoped| Activity (survives rotate) | ActivityRetainedComponent  |
| @ViewModelScoped       | one ViewModel              | ViewModelComponent         |
| @ActivityScoped        | one Activity               | ActivityComponent          |
| @FragmentScoped        | one Fragment               | FragmentComponent          |
| @ViewScoped            | one View                   | ViewComponent              |
| @ServiceScoped         | one Service                | ServiceComponent           |
| (no scope)             | new instance every time    | -                          |
+------------------------+----------------------------+----------------------------+
```

---

###### Component Hierarchy

```text
+------------------------------+
|  SingletonComponent          |   @Singleton (whole app)
+------------------------------+
|  ActivityRetainedComponent   |   @ActivityRetainedScoped (survives rotation)
+------------------------------+
|  ViewModelComponent          |   @ViewModelScoped
+------------------------------+
|  ActivityComponent           |   @ActivityScoped
+------------------------------+
|  FragmentComponent           |   @FragmentScoped
+------------------------------+
|  ViewComponent               |   @ViewScoped
+------------------------------+
```

- Lower components can use bindings from higher ones, not the other way around

---

###### How it Works
- Hilt creates a component when its host is created (App/Activity/Fragment/etc.)
- A scoped binding is built once per component instance
- Every injection inside that component returns the same object
- When the host is destroyed, the component and its scoped objects are released

---

###### When to Use Each Scope
- @Singleton -> Retrofit, OkHttp, Room database, app-wide config
- @ActivityRetainedScoped -> data shared across rotation but not the whole app
- @ViewModelScoped -> helpers used only by one ViewModel
- @ActivityScoped -> Activity-specific UI controllers, navigators
- @FragmentScoped -> Fragment-specific helpers
- No scope -> stateless utilities (formatters, mappers)

---

###### Example (Step-by-step)

**Step 1 — Singleton: one OkHttp for the whole app**

```kotlin
// @Singleton -> built once, lives until app dies
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    @Singleton                                // cache inside SingletonComponent
    fun provideOkHttp(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

- Same OkHttpClient injected everywhere

**Step 2 — ViewModel-scoped: helper only this VM uses**

```kotlin
// @ViewModelScoped -> one per ViewModel instance
@Module
@InstallIn(ViewModelComponent::class)
object SearchModule {

    @Provides
    @ViewModelScoped
    fun provideSearchHistory(): SearchHistory =
        SearchHistory()
}

@HiltViewModel
class SearchTaskViewModel @Inject constructor(
    private val history: SearchHistory        // same SearchHistory for this VM
) : ViewModel()
```

- A different VM gets its own SearchHistory

**Step 3 — Unscoped: new instance every time**

```kotlin
// No scope -> Hilt creates a fresh one on each injection
@Module
@InstallIn(SingletonComponent::class)
object FormatModule {

    @Provides
    fun provideDateFormatter(): DateFormatter =
        DateFormatter()
}
```

- Cheap, stateless helpers don't need to be cached

---

###### Full Example

```kotlin
// =========================================================
// Singleton: shared by the whole app
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

    @Provides @Singleton
    fun provideApiService(retrofit: Retrofit): ApiService =
        retrofit.create(ApiService::class.java)
}

// =========================================================
// ViewModel-scoped: lives one ViewModel
// =========================================================
@Module
@InstallIn(ViewModelComponent::class)
object SearchModule {

    @Provides
    @ViewModelScoped
    fun provideSearchHistory(): SearchHistory = SearchHistory()
}

class SearchHistory {
    private val items = mutableListOf<String>()
    fun add(q: String) { items += q }
    fun all(): List<String> = items
}

// =========================================================
// Repository: @Singleton (one for the whole app)
// =========================================================
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds @Singleton
    abstract fun bindRepo(impl: TaskRepositoryImpl): TaskRepository
}

// =========================================================
// ViewModel: pulls a singleton repo + a VM-scoped helper
// =========================================================
@HiltViewModel
class SearchTaskViewModel @Inject constructor(
    private val repository: TaskRepository,   // same instance everywhere
    private val history: SearchHistory        // unique to this ViewModel
) : ViewModel()
```

---

###### Common Mistakes
- BAD: making everything `@Singleton` -> wastes memory, causes leaks
- BAD: scoping to the wrong component (e.g. @ActivityScoped binding used in a Fragment-only module)
- BAD: forgetting `@InstallIn` -> binding never registers
- BAD: holding a Context-bound object as `@Singleton` -> memory leak
- BAD: @ViewModelScoped object kept beyond the ViewModel -> leak

---

###### Weak Area Clarification
- Confusion: "Should I scope this or not?"
- Why: scoping looks like a free win
- Resolution: scope only when (a) the object is expensive to build, or (b) state must be shared. Stateless helpers stay unscoped.

---

###### Common Follow-up Traps
- Q: What's the difference between @ActivityScoped and @ActivityRetainedScoped?
  A: @ActivityScoped dies on rotation; @ActivityRetainedScoped survives rotation (same lifetime as a ViewModel)
- Q: Can a @Singleton depend on an @ActivityScoped object?
  A: No — a longer-lived object cannot reach into a shorter-lived component
- Q: Does @HiltViewModel need @ViewModelScoped?
  A: No — @HiltViewModel already lives one-per-ViewModel; the scope is for helpers inside the ViewModelComponent

---

###### Memory Hook
- Scope = lifetime sticker. Bigger sticker -> longer life -> more sharing.

---

###### Key Rule
- Pick the smallest scope that still fits the object's natural lifetime. Default to no scope.

---

###### Related
- [[DI - Overview]]
- [[Hilt - DI Library]]
- [[Hilt - Architecture Layers]]
