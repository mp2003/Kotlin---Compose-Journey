---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- When a DAO function returns `Flow`, Room automatically re-emits the latest data every time the table changes, so the UI stays live without any manual refresh.

---

###### Definition
- Flow-from-DB is the pattern of returning a `Flow` from a `@Query` so the UI observes the database like a stream instead of a one-shot fetch.

---

###### Real-World Analogy
- One-shot fetch -> you call the front desk, they give you a list, you hang up
- Flow-from-DB -> you stay on the line — the front desk calls you back every time the list changes

---

###### What
- A `@Query` that returns `Flow<List<T>>` or `Flow<T?>` stays active
- Every INSERT / UPDATE / DELETE on that table triggers a new emission
- The ViewModel collects it into a `StateFlow`
- Compose recomposes when the StateFlow changes

---

###### Why
- No manual refresh needed — the UI is always showing the latest data
- One line in the DAO wires up a live data pipeline all the way to the screen
- The offline-first pattern — data comes from Room, network just updates Room

---

###### Core Concepts
- `Flow<List<T>>` from DAO -> live query, re-emits on table change
- `.stateIn(scope, SharingStarted.WhileSubscribed(), emptyList())` -> converts the Flow to a StateFlow the UI can collect
- `collectAsState()` in Compose -> subscribes and triggers recomposition

---

###### How it Works

```text
Table changes (INSERT / DELETE)
        |
        v
DAO Flow emits new list
        |
        v
Repository passes Flow to ViewModel
        |
        v
ViewModel converts to StateFlow (.stateIn)
        |
        v
Compose collects with collectAsState()
        |
        v
UI recomposes with new data
```

---

###### Syntax

```kotlin
// DAO — returns Flow, no suspend (stays live)
@Query("SELECT * FROM tickets ORDER BY dateMillis DESC")
fun observeAll(): Flow<List<TicketEntity>>

// ViewModel — convert to StateFlow so Compose can collect it
val tickets: StateFlow<List<TicketEntity>> = repository
    .observeAll()
    .stateIn(
        scope = viewModelScope,               // cancelled when ViewModel is cleared
        started = SharingStarted.WhileSubscribed(5000), // keeps alive 5s after last collector
        initialValue = emptyList()            // what the UI sees before first emission
    )

// Compose — collect and trigger recomposition
val tickets by viewModel.tickets.collectAsState()
```

---

###### Common Mistakes
- BAD: making the DAO read `suspend` -> use `Flow`, not suspend, for live queries
- BAD: calling `dao.observeAll()` directly on the main thread -> collect in a ViewModel scope
- BAD: forgetting `initialValue` in `stateIn` -> UI has nothing to show before first DB read

---

###### Memory Hook
- Flow-from-DB = stay on the line. The DB calls you back every time something changes.

---

###### Key Rule
- Reads that need to stay live return `Flow`. The ViewModel converts to `StateFlow`. Compose collects.

---

###### Related
- [[00 - Room - Overview]]
- [[04 - Room - Repository]]
- [[06 - Room - ServiceLocator (manual DI)]]
