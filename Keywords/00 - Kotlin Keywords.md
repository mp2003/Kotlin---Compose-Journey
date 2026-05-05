###### Elevator Pitch
- This is your Kotlin cheat sheet — every keyword you will see in Android code, what it does, and the exact syntax to write it.

---

###### Definition
- A reference table of Kotlin language keywords with one-line meanings and minimal code examples

---

###### Real-World Analogy
- A dictionary for a new language — you see the word in a sentence, look it up here, and instantly know what it means

---

###### Keywords Table

| Keyword | What it does | When you write it |
|---|---|---|
| `data class` | auto-generates equals / hashCode / copy / toString | any class that just holds data (UiState, model) |
| `sealed class` | restricted hierarchy — all subclasses in the same file | UiIntent, UiEffect, error types |
| `sealed interface` | same as sealed class but allows multiple inheritance | prefer over sealed class for intents/effects |
| `object` | singleton — one instance for the whole app | utility objects, companion members, Hilt modules |
| `abstract class` | cannot be instantiated — must be extended | Hilt `@Binds` modules, base classes |
| `interface` | a contract — defines what a class must do, not how | repository, api service, any boundary |
| `val` | read-only reference — cannot be reassigned | almost always, unless you need to reassign |
| `var` | mutable reference — can be reassigned | only when reassignment is genuinely needed |
| `fun` | declares a function | everywhere |
| `suspend` | marks a function that can pause and resume | any function that calls a coroutine or does I/O |
| `override` | replaces a function from a parent class or interface | implementing interface methods |
| `companion object` | static-like block inside a class | factory methods, constants |
| `when` | pattern-match — Kotlin's switch statement | handling sealed class branches (intents, effects) |
| `is` | type check — true if the object is that type | inside `when` blocks |
| `by` | delegation — lets another object do the work | `val state by vm.state.collectAsState()` |
| `.copy()` | creates a new data class with one field changed | updating UiState immutably |
| `init` | runs when the class is first created | setup logic in ViewModel/class |
| `private` | only visible inside this class | `_state`, `_effect` in ViewModel |
| `constructor` | defines how a class is created | `@Inject constructor(...)` in Hilt |

---

###### Syntax

```kotlin
// data class — holds state, gets copy() for free
data class UiState(
    val isLoading: Boolean = false,   // default value
    val items: List<String> = emptyList()
)

// sealed interface — restricted set of types
sealed interface UiIntent {
    data class Search(val query: String) : UiIntent   // carries data
    object Refresh : UiIntent                          // no data
}

// object — singleton
object AppConfig {
    val baseUrl = "https://api.example.com"
}

// abstract class — cannot be used directly
abstract class BaseModule {
    abstract fun bind(): Repository
}

// interface — defines the contract
interface TaskRepository {
    fun getTasks(): List<String>
    suspend fun loadFromApi(): List<String>   // suspend = can pause
}

// val vs var
val name = "Milind"        // cannot reassign
var count = 0              // can reassign: count = 1

// when — matches branches
when (intent) {
    is UiIntent.Search  -> search(intent.query)   // is = type check
    is UiIntent.Refresh -> refresh()
}

// companion object — static-like
class TaskViewModel {
    companion object {
        const val TAG = "TaskViewModel"   // accessible without an instance
    }
}

// by — delegation
val state by vm.state.collectAsState()   // 'by' delegates the getter to collectAsState()

// copy — immutable update
val newState = currentState.copy(isLoading = true)   // only isLoading changes
```

---

###### Common Mistakes
- BAD: using `var` everywhere — use `val` by default, only `var` when you must reassign
- BAD: using `class` instead of `data class` for state — you lose `.copy()`
- BAD: using `sealed class` when `sealed interface` works — interface is more flexible
- BAD: forgetting `suspend` on a function that calls another `suspend` function

---

###### Memory Hook
- `val` = value (locked), `var` = variable (changeable), `suspend` = pauseable, `sealed` = locked family

---

###### Key Rule
- Default to `val`, `sealed interface`, `data class` — change only when you have a reason

---

###### Related
- [[01 - Android & Hilt Keywords]]
