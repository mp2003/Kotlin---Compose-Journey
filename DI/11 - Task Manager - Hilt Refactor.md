---
up: "[[00 - DI - Overview]]"
---

###### Elevator Pitch
- Refactoring the Task Manager with Hilt removes all manual `new` keywords from the UI/ViewModel layers and lets Hilt build the entire dependency tree (UI -> ViewModel -> Repository -> ApiService) automatically.

---

###### Definition
- A real, end-to-end MVI screen where every dependency is injected by Hilt instead of created by hand

---

###### Real-World Analogy
- Old way -> a chef who farms his own vegetables, raises his own chickens, then cooks
- Hilt way -> the chef just cooks; ingredients arrive at the kitchen door already prepped

---

###### What
- One Compose screen (`SearchTaskScreen`) backed by an MVI ViewModel
- ViewModel does NOT create the Repository
- Repository does NOT create the ApiService
- Hilt wires the whole chain at runtime

---

###### Why
- Removes manual object creation from UI/ViewModel
- Lets us swap implementations (Fake -> Real API) by changing ONE qualifier
- Makes the screen testable -> pass a fake repo in tests
- Matches real production app layout

---

###### Core Concepts
- `@HiltAndroidApp` -> turns Application into Hilt's root container
- `@AndroidEntryPoint` -> lets an Activity receive injections
- `@HiltViewModel` -> lets a ViewModel receive injections
- `@Inject constructor` -> "Hilt, please build me"
- `@Module + @Binds` -> "interface -> implementation" rule
- `@Module + @Provides` -> "build this object yourself" rule (for third-party)
- `@Qualifier` -> tag used to pick between same-type implementations
- Link: [[00 - DI - Overview]], [[03 - Hilt - DI Library]]

---

###### Project Layout

```text
composeplayground/
+-- MyApp.kt                 (@HiltAndroidApp)
+-- MainActivity.kt          (@AndroidEntryPoint)
+-- domain/
|     +-- TaskRepository.kt        (interface)
+-- data/
|     +-- TaskRepositoryImpl.kt    (@Inject)
|     +-- remote/
|           +-- ApiService.kt      (interface + qualifiers + fakes)
+-- di/
|     +-- NetworkModule.kt         (@Provides ApiService)
|     +-- RepositoryModule.kt      (@Binds TaskRepository)
+-- ui/search/
      +-- SearchTaskScreen.kt
      +-- SearchTaskViewModel.kt   (@HiltViewModel)
      +-- SearchTaskState.kt
      +-- SearchTaskIntent.kt
      +-- SearchTaskEffect.kt
```

---

###### Layered Diagram

```text
+-------------------------------+
|  UI (Compose)                 |   SearchTaskScreen
+-------------------------------+
|  ViewModel (MVI)              |   SearchTaskViewModel
+-------------------------------+
|  Domain (interface)           |   TaskRepository
+-------------------------------+
|  Data (implementation)        |   TaskRepositoryImpl
+-------------------------------+
|  Remote (interface + impls)   |   ApiService / FakeApiService etc.
+-------------------------------+
```

---

###### How it Works
- App starts -> `@HiltAndroidApp` creates the root container
- Activity is opened -> `@AndroidEntryPoint` lets it inject things
- Compose calls `hiltViewModel()` -> Hilt creates `SearchTaskViewModel`
- ViewModel needs `TaskRepository` -> Hilt sees `@Binds` rule -> creates `TaskRepositoryImpl`
- Impl needs `ApiService` with `@SearchApiResult` qualifier -> Hilt finds `@Provides` -> returns `SearchApiService`
- Whole chain built automatically, top to bottom

---

###### ASCII Flowchart

```text
[ SearchTaskScreen ]
        |
        v   hiltViewModel()
[ SearchTaskViewModel ] -- @Inject --> [ TaskRepository ]
                                              |
                                              v   @Binds
                                       [ TaskRepositoryImpl ]
                                              |
                                              v   @Inject + @SearchApiResult
                                       [ ApiService ]
                                              |
                                              v   @Provides
                                       [ SearchApiService ]
```

---

###### Example (Step-by-step)

**Step 1 — Make the Application a Hilt root**

```kotlin
// Tells Hilt: "the whole app graph lives here"
@HiltAndroidApp
class MyApp : Application()
```

- Without this, nothing else in the chain works

**Step 2 — Mark the Activity as an entry point**

```kotlin
// Lets this Activity (and its Composables) receive injections
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Scaffold { _ -> SearchTaskScreen() }
        }
    }
}
```

- Activity is the door -> `@AndroidEntryPoint` opens it for Hilt

**Step 3 — Define the domain interface**

```kotlin
// Pure abstraction, no Android, no Hilt -> easy to test
interface TaskRepository {
    fun getTasks(): List<String>
    fun search(query: String): List<String>
}
```

- ViewModel depends on this interface, not the impl

**Step 4 — Define the API interface + qualifiers + fakes**

```kotlin
// Many possible implementations of the same type -> need a qualifier
interface ApiService {
    fun getTasks(): List<String>
}

@Qualifier @Retention(AnnotationRetention.BINARY) annotation class FakeApi
@Qualifier @Retention(AnnotationRetention.BINARY) annotation class RealApi
@Qualifier @Retention(AnnotationRetention.BINARY) annotation class SearchApiResult

class FakeApiService   @Inject constructor() : ApiService { /* ... */ }
class RealApiService   @Inject constructor() : ApiService { /* ... */ }
class SearchApiService @Inject constructor() : ApiService {
    override fun getTasks() = listOf("Learn Kotlin", "Build App", "Read Docs")
}
```

- Same type, three impls -> qualifiers tell Hilt which one to use

**Step 5 — Implement the repository**

```kotlin
// @Inject -> "Hilt, you can build me"
// @SearchApiResult -> "and inject THIS specific ApiService"
class TaskRepositoryImpl @Inject constructor(
    @SearchApiResult private val api: ApiService
) : TaskRepository {
    override fun getTasks(): List<String> = api.getTasks()
    override fun search(query: String): List<String> =
        api.getTasks().filter { it.contains(query, ignoreCase = true) }
}
```

- Repo never builds the API -> Hilt hands it over

**Step 6 — Bind the interface to its implementation (@Binds)**

```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {
    // "When someone asks for TaskRepository, give them TaskRepositoryImpl"
    @Binds
    @Singleton
    abstract fun bindTaskRepository(impl: TaskRepositoryImpl): TaskRepository
}
```

- @Binds is metadata only -> compile-time wiring, zero runtime cost

**Step 7 — Provide the API objects (@Provides)**

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    // @Provides because tomorrow this becomes Retrofit.Builder()...build()
    @FakeApi          @Singleton @Provides fun provideFakeApi(): ApiService = FakeApiService()
    @RealApi          @Singleton @Provides fun provideRealApi(): ApiService = RealApiService()
    @SearchApiResult  @Singleton @Provides fun provideSearchApi(): ApiService = SearchApiService()
}
```

- @Provides used here because real Retrofit instances cannot be `@Inject`-built

**Step 8 — Make the ViewModel injectable**

```kotlin
// @HiltViewModel -> Hilt knows how to create this ViewModel
// @Inject constructor -> Hilt fills in the Repository
@HiltViewModel
class SearchTaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel() {

    private val _state = MutableStateFlow(SearchTaskState())
    val state = _state.asStateFlow()

    private val _effect = MutableSharedFlow<SearchTaskEffect>()
    val effect = _effect.asSharedFlow()

    fun onIntent(intent: SearchTaskIntent) {
        when (intent) {
            is SearchTaskIntent.Search -> search(intent.query)
        }
    }

    private fun search(query: String) {
        val results = repository.search(query)
        _state.update { it.copy(query = query, tasks = results) }
        if (query.isNotEmpty()) {
            viewModelScope.launch {
                _effect.emit(SearchTaskEffect.ShowToast("Searching ..."))
            }
        }
    }
}
```

- ViewModel never says `TaskRepositoryImpl()` -> only the interface

**Step 9 — Use `hiltViewModel()` in the Composable**

```kotlin
@Composable
fun SearchTaskScreen(
    vm: SearchTaskViewModel = hiltViewModel()   // Hilt creates the VM here
) {
    val state by vm.state.collectAsState()
    val context = LocalContext.current

    LaunchedEffect(Unit) {
        vm.effect.collect { effect ->
            when (effect) {
                is SearchTaskEffect.ShowToast ->
                    Toast.makeText(context, effect.message, Toast.LENGTH_SHORT).show()
            }
        }
    }

    SearchTaskContent(state = state, onIntent = vm::onIntent)
}
```

- `hiltViewModel()` is the bridge from Compose into the Hilt graph

---

###### Full Example

```kotlin
// =========================================================
// 1. Application root
// =========================================================
@HiltAndroidApp
class MyApp : Application()

// =========================================================
// 2. Activity entry point
// =========================================================
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { Scaffold { _ -> SearchTaskScreen() } }
    }
}

// =========================================================
// 3. Domain interface
// =========================================================
interface TaskRepository {
    fun getTasks(): List<String>
    fun search(query: String): List<String>
}

// =========================================================
// 4. API interface + qualifiers + fakes
// =========================================================
interface ApiService { fun getTasks(): List<String> }

@Qualifier @Retention(AnnotationRetention.BINARY) annotation class FakeApi
@Qualifier @Retention(AnnotationRetention.BINARY) annotation class RealApi
@Qualifier @Retention(AnnotationRetention.BINARY) annotation class SearchApiResult

class FakeApiService   @Inject constructor() : ApiService {
    override fun getTasks() = listOf("Fake API Task 1", "Fake API Task 2")
}
class RealApiService   @Inject constructor() : ApiService {
    override fun getTasks() = listOf("Real API Task 1", "Real API Task 2")
}
class SearchApiService @Inject constructor() : ApiService {
    override fun getTasks() = listOf("Learn Kotlin", "Build App", "Read Docs")
}

// =========================================================
// 5. Repository implementation
// =========================================================
class TaskRepositoryImpl @Inject constructor(
    @SearchApiResult private val api: ApiService
) : TaskRepository {
    override fun getTasks() = api.getTasks()
    override fun search(query: String) =
        api.getTasks().filter { it.contains(query, ignoreCase = true) }
}

// =========================================================
// 6. RepositoryModule -> @Binds (interface -> impl)
// =========================================================
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {
    @Binds @Singleton
    abstract fun bindTaskRepository(impl: TaskRepositoryImpl): TaskRepository
}

// =========================================================
// 7. NetworkModule -> @Provides (build the object ourselves)
// =========================================================
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @FakeApi         @Singleton @Provides fun provideFakeApi():   ApiService = FakeApiService()
    @RealApi         @Singleton @Provides fun provideRealApi():   ApiService = RealApiService()
    @SearchApiResult @Singleton @Provides fun provideSearchApi(): ApiService = SearchApiService()
}

// =========================================================
// 8. MVI pieces
// =========================================================
data class SearchTaskState(
    val tasks: List<String> = emptyList(),
    val query: String = ""
)

sealed interface SearchTaskIntent {
    data class Search(val query: String) : SearchTaskIntent
}

sealed interface SearchTaskEffect {
    data class ShowToast(val message: String) : SearchTaskEffect
}

// =========================================================
// 9. ViewModel
// =========================================================
@HiltViewModel
class SearchTaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel() {

    private val _state = MutableStateFlow(SearchTaskState())
    val state = _state.asStateFlow()

    private val _effect = MutableSharedFlow<SearchTaskEffect>()
    val effect = _effect.asSharedFlow()

    fun onIntent(intent: SearchTaskIntent) = when (intent) {
        is SearchTaskIntent.Search -> search(intent.query)
    }

    private fun search(query: String) {
        val results = repository.search(query)
        _state.update { it.copy(query = query, tasks = results) }
        if (query.isNotEmpty()) {
            viewModelScope.launch {
                _effect.emit(SearchTaskEffect.ShowToast("Searching ..."))
            }
        }
    }
}

// =========================================================
// 10. Compose screen
// =========================================================
@Composable
fun SearchTaskScreen(
    vm: SearchTaskViewModel = hiltViewModel()
) {
    val state by vm.state.collectAsState()
    val context = LocalContext.current

    LaunchedEffect(Unit) {
        vm.effect.collect { effect ->
            when (effect) {
                is SearchTaskEffect.ShowToast ->
                    Toast.makeText(context, effect.message, Toast.LENGTH_SHORT).show()
            }
        }
    }

    Column(modifier = Modifier.safeContentPadding()) {
        TextField(
            value = state.query,
            onValueChange = { vm.onIntent(SearchTaskIntent.Search(it)) },
            modifier = Modifier.fillMaxWidth()
        )
        Spacer(Modifier.height(16.dp))
        if (state.tasks.isEmpty()) Text("No results found")
        else LazyColumn {
            items(state.tasks) { Text(it); Spacer(Modifier.height(8.dp)) }
        }
    }
}
```

---

###### Common Mistakes
- BAD: forgetting `@HiltAndroidApp` on the Application class -> nothing injects
- BAD: forgetting `@AndroidEntryPoint` on the Activity -> `hiltViewModel()` crashes
- BAD: same-type providers without qualifiers -> "duplicate binding" compile error
- BAD: building the repo with `TaskRepositoryImpl()` somewhere -> defeats DI
- BAD: using `@Binds` on something that needs runtime construction (Retrofit) -> use `@Provides`

---

###### Weak Area Clarification
- Confusion: when do I use @Binds vs @Provides?
- Why: both "give Hilt an object", but they work differently
- Resolution: @Binds = "I have a constructor-injected class, just hand it over as the interface"; @Provides = "I need to build this object myself" (Retrofit, OkHttp, anything third-party)

---

###### Common Follow-up Traps
- Q: Can the ViewModel inject `TaskRepositoryImpl` directly?
  A: It can, but never do it -> always depend on the `TaskRepository` interface
- Q: Why three qualifiers if we only use one?
  A: To prove the swap point -> change `@SearchApiResult` to `@RealApi` in the repo and a different impl is injected with zero other code changes
- Q: Why is `NetworkModule` an `object` but `RepositoryModule` is `abstract class`?
  A: `@Provides` needs real function bodies (object); `@Binds` is metadata-only and requires `abstract`

---

###### Memory Hook
- App -> Activity -> ViewModel -> Repo -> Api -> Hilt builds the chain, you just ask

---

###### Key Rule
- The UI never builds the ViewModel, the ViewModel never builds the Repo, the Repo never builds the Api -> Hilt does all three

---

###### Related
- [[00 - DI - Overview]]
- [[03 - Hilt - DI Library]]
- [[08 - Qualifiers]]
