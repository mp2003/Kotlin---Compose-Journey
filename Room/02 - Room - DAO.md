---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- A DAO is an interface marked with `@Dao` that lists the database operations as functions, and Room writes the actual SQL code for you.

---

###### Definition
- A DAO (Data Access Object) is an interface that declares the read and write operations on a table.

---

###### Real-World Analogy
- DAO -> the clerk at the filing cabinet
- You hand the clerk a request (function call)
- The clerk runs the SQL and brings back the result

---

###### What
- `@Dao interface` -> you declare functions, never write the bodies
- Reads -> return `Flow`, stay live
- Writes -> are `suspend`, run once

---

###### Why
- Type-safe -> SQL errors caught at compile time
- No boilerplate -> Room generates the implementation via KSP
- Flow reads -> UI auto-updates when the table changes

---

###### Core Concepts
- `@Query` -> you write raw SQL, Room generates the code
- `:param` -> binds a function parameter into the SQL
- `@Insert` / `@Update` / `@Delete` -> auto-generated, no SQL needed
- Nullable return -> a read that might find nothing returns `T?`

---

###### How it Works
- Declare the function signature + annotation
- KSP reads it and writes the real SQL implementation class
- At runtime the database hands you that generated implementation
- A read returning `Flow` re-emits every time the table changes

---

###### Syntax
- `@Dao` -> marks the interface as the query set
- `@Query("SQL")` -> raw SQL you write yourself
- `:id` -> binds a Kotlin parameter into the SQL
- `@Insert` -> auto INSERT, can return the new row id (`Long`)
- `suspend` -> a write, runs off the main thread
- `Flow<...>` -> a read, auto re-emits on change

```kotlin
@Dao
interface TicketDao {
    // READ all -> Flow, no suspend (stays live, newest first)
    @Query("SELECT * FROM tickets ORDER BY dateMillis DESC")
    fun observeAll(): Flow<List<TicketEntity>>

    // READ one -> nullable, the row might not exist
    @Query("SELECT * FROM tickets WHERE id = :id")
    fun observeById(id: Long): Flow<TicketEntity?>

    // WRITE -> suspend, returns the new id
    @Insert
    suspend fun insert(ticket: TicketEntity): Long

    // WRITE -> suspend, deletes by id
    @Query("DELETE FROM tickets WHERE id = :id")
    suspend fun deleteById(id: Long)
}
```

---

###### Common Mistakes
- BAD: making a read `suspend` -> use `Flow` so the UI stays live
- BAD: returning non-null for a lookup that can miss -> use `T?`
- BAD: writing a class instead of an interface -> DAO is an interface
- BAD: importing a class from the same package -> not needed

---

###### Weak Area Clarification
- Why Flow for reads but suspend for writes?
- A read should keep watching the table -> Flow re-emits on every change.
- A write happens once -> suspend runs it off the main thread and returns.

---

###### Memory Hook
- Reads = Flow (live). Writes = suspend (one-shot).

---

###### Key Rule
- A read that might find nothing must return a nullable type.

---

###### Related
- [[00 - Room - Overview]]
- [[01 - Room - Entity]]
- [[03 - Room - Database]]
