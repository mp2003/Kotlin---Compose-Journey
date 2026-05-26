---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- A ServiceLocator is a plain `object` that builds and hands out the repository so every ViewModel gets the same instance without Hilt.

---

###### Definition
- A ServiceLocator is a manual dependency provider — a single place that knows how to construct each dependency and gives it out on request.

---

###### Real-World Analogy
- Hilt -> a vending machine that auto-delivers dependencies
- ServiceLocator -> a supply room with one person inside — you knock, they hand you what you need
- Same result, more manual work

---

###### What
- A Kotlin `object` (singleton by default)
- One function per dependency it provides
- Builds the full chain: Database -> DAO -> Repository

---

###### Why
- No Hilt setup needed — simpler for learning
- One place to change when you swap a dependency
- Easy to replace with Hilt later — it's a one-file change

---

###### Syntax

```kotlin
// object = singleton, no instantiation needed
object ServiceLocator {

    // builds the full chain and returns the repository
    fun provideRepository(context: Context): TicketRepository =
        TicketRepository(
            AppDatabase.getInstance(context).ticketDao()
        )
}

// Usage in a ViewModel factory or Activity:
val repository = ServiceLocator.provideRepository(applicationContext)
```

---

###### Common Mistakes
- BAD: calling `ServiceLocator.provideRepository(activityContext)` -> use `applicationContext`
- BAD: building the repository inside the ViewModel -> breaks separation of concerns

---

###### Memory Hook
- ServiceLocator = supply room. Knock and it hands you the repository.

---

###### Key Rule
- ServiceLocator is a stepping stone — build it now, replace with Hilt when ready.

---

###### Related
- [[00 - Room - Overview]]
- [[03 - Room - Database]]
- [[04 - Room - Repository]]
