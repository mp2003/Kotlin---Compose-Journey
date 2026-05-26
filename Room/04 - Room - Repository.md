---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- A repository is a class that wraps the DAO and gives the ViewModel a clean way to access data without ever knowing Room exists.

---

###### Definition
- A repository is a thin data-access class that sits between the ViewModel and the DAO, hiding database details from the UI layer.

---

###### Real-World Analogy
- DAO -> the clerk who files and fetches from the cabinet
- Repository -> the front desk — you ask the front desk, they talk to the clerk
- ViewModel -> you, the person who needs the data
- UI -> the screen that shows the result

---

###### What
- A plain Kotlin class — no Room annotations needed
- Takes the DAO as a constructor parameter
- One function per DAO operation — each just delegates to the DAO
- The ViewModel only ever calls the repository, never the DAO directly

---

###### Why
- ViewModel stays clean — no Room imports, no SQL knowledge
- One place to swap the data source later (e.g. add a network call alongside Room)
- Same pattern as your Networking repository — reusable mental model

---

###### Core Concepts
- Constructor injection -> the DAO is passed in, not created inside
- Delegation -> each function calls the matching DAO function and returns the result
- Thin layer -> no extra logic here, just a seam

---

###### How it Works
- ViewModel calls `repository.observeTickets()`
- Repository calls `dao.observeAll()` and returns the Flow
- ViewModel never touches the DAO directly

---

###### Syntax

```kotlin
// Plain class — no annotations
// DAO goes in the constructor (constructor injection)
class TicketRepository(private val dao: TicketDao) {

    // READ — return the Flow straight from the DAO, no suspend needed
    fun observeTickets(): Flow<List<TicketEntity>> = dao.observeAll()

    // READ — one ticket by id, nullable
    fun observeTicket(id: Long): Flow<TicketEntity?> = dao.observeById(id)

    // WRITE — suspend because the DAO function is suspend
    suspend fun addTicket(ticket: TicketEntity): Long = dao.insert(ticket)

    // WRITE — suspend, returns nothing
    suspend fun deleteTicket(id: Long) = dao.deleteById(id)
}
```

---

###### Common Mistakes
- BAD: creating the DAO inside the repository -> hard to test, breaks DI
- BAD: putting business logic in the repository -> keep it thin, logic lives in the ViewModel
- BAD: calling the DAO directly from the ViewModel -> always go through the repository

---

###### Memory Hook
- Repository = front desk. ViewModel asks the front desk. Front desk talks to the clerk (DAO).

---

###### Key Rule
- Each repository function does one thing — calls the matching DAO function and returns the result.

---

###### Related
- [[00 - Room - Overview]]
- [[03 - Room - Database]]
- [[05 - Room - Flow from Database]]
