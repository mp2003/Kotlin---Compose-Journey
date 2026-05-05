---
up: "[[00 - DI - Overview]]"
---

###### Definition
- Hilt is a dependency injection library that automatically provides dependencies in Android apps
  
###### What
- Manages object creation for you
- Connects dependencies across layers
- Built on top of Dagger
- Works with Android lifecycle (Activity, ViewModel, etc.)

###### Why
- Removes manual object creation
- Reduces boilerplate
- Ensures correct lifecycle handling
- Makes DI easy to use in Android

###### Core Concepts
- @HiltAndroidApp → starts DI container
- @AndroidEntryPoint → allows injection into Android classes
- @Inject → tells Hilt how to create object
- @Module → where you define dependencies manually
- @Binds → bind interface to implementation
- @Provides → create objects manually (Retrofit, etc.)
- Scopes → control lifecycle (Singleton, ViewModelScoped)

###### How it Works (Flow)
- App starts → Hilt initializes
- Android class (Activity/ViewModel) requests dependency
- Hilt checks dependency graph
- If available → provides it
- If not → creates it using @Inject or Module
- Injects into class

###### Setup (Minimum Required)

Application:
```kotlin
@HiltAndroidApp
class MyApp : Application()
````

Activity:

```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity()
```

###### **Constructor Injection (First Step)**

```kotlin
class TaskRepository @Inject constructor()

@HiltViewModel
class TaskViewModel @Inject constructor(
    private val repository: TaskRepository
) : ViewModel()
```

###### **Interface + Binding**

```kotlin
interface TaskRepository

class TaskRepositoryImpl @Inject constructor() : TaskRepository
```

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

###### **Provide External Libraries**

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder().build()
    }
}
```

###### **Compose Usage**

```kotlin
@Composable
fun TaskScreen(
    viewModel: TaskViewModel = hiltViewModel()
)
```

###### **What to Look For in Code**

- @Inject on constructors
- @HiltViewModel on ViewModel
- @AndroidEntryPoint on Activity
- Interfaces instead of implementations
- Modules for bindings/provides

###### **Common Flow (Important)**

- UI → ViewModel (injected)
- ViewModel → Repository (injected)
- Repository → API/DB (injected)

###### **When to Use @Binds**

- When using interface + implementation
- No object creation logic needed

###### **When to Use @Provides**

- When object needs manual creation (Retrofit, OkHttp, etc.)

###### **Common Mistakes**

- Forgetting @AndroidEntryPoint
- Creating objects manually alongside Hilt
- Not binding interfaces
- Wrong scope usage

---

###### Keywords Used

| Keyword | What it does |
|---------|--------------|
| `@HiltAndroidApp` | Marks the Application class as Hilt's root DI container |
| `class MyApp : Application()` | Custom Application subclass required for Hilt initialization |
| `@AndroidEntryPoint` | Allows an Activity or Fragment to receive Hilt injections |
| `class MainActivity : ComponentActivity()` | Activity that Hilt can inject into |
| `@Inject constructor()` | Tells Hilt it can build this class using its constructor |
| `@HiltViewModel` | Marks the ViewModel so Hilt (not Android) creates it |
| `private val repository: TaskRepository` | Declares dependency on the interface; Hilt resolves the impl |
| `interface TaskRepository` | Abstraction the ViewModel depends on |
| `class TaskRepositoryImpl @Inject constructor()` | Concrete implementation Hilt can build |
| `@Module` | Container of Hilt binding and provide rules |
| `@InstallIn(SingletonComponent::class)` | Installs rules into the app-wide component |
| `abstract class RepositoryModule` | Required abstract class for @Binds rules |
| `@Binds` | Wires an interface to its implementation at compile time |
| `abstract fun bindRepo(impl: TaskRepositoryImpl): TaskRepository` | The actual interface-to-impl binding declaration |
| `object NetworkModule` | Singleton object holding @Provides functions |
| `@Provides` | Marks a function that Hilt calls to build an object |
| `fun provideRetrofit(): Retrofit` | Returns a built Retrofit instance for Hilt to inject |
| `@Composable` | Marks a function as a Compose UI component |
| `hiltViewModel()` | Obtains a Hilt-injected ViewModel inside a composable |
