---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- `AppDatabase` is the one class that assembles your tables and DAOs into an actual SQLite database file, and the singleton pattern ensures the whole app shares a single connection.

---

###### Definition
- An `@Database` abstract class is the Room entry point — it declares which tables exist and exposes the DAOs that query them.

---

###### Real-World Analogy
- `AppDatabase` -> the filing cabinet
- `@Database(entities = [...])` -> the label on the cabinet listing what folders are inside
- `getInstance()` -> the rule "there is only ONE cabinet in the office, not one per person"
- `synchronized` -> the lock on the cabinet so two people can't both open it at the same time

---

###### What
- One `abstract class` annotated with `@Database`
- Lists all `@Entity` tables it holds
- Declares `abstract fun` for each DAO -> Room writes the body
- A `companion object` builds and holds the single instance

---

###### Why
- Building two database instances wastes resources and can corrupt data
- Singleton ensures the whole app shares one connection
- `abstract` lets Room generate the real implementation via KSP

---

###### Core Concepts
- `@Database` -> declares the DB, its tables, and its version
- `version` -> bump this when you change a table's columns (migrations)
- `exportSchema = false` -> skip writing a schema-history JSON (fine for learning)
- `companion object` -> class-level area, called without creating an instance
- `@Volatile` -> the field update is immediately visible to all threads
- `synchronized` -> only one thread can run the block at a time
- Double-checked locking -> check twice (outside + inside the lock) to avoid building twice

---

###### How it Works
- First call to `getInstance` -> `instance` is null -> enter `synchronized` -> build DB -> store in `instance` -> return it
- Every call after -> `instance` is not null -> return it immediately (skips the lock)
- Two threads at the same time -> only one gets through `synchronized` -> other waits -> finds it already built on second check

---

###### Syntax

```kotlin
@Database(
    entities = [TicketEntity::class], // which tables this DB holds
    version = 1,                      // schema version, bump on column changes
    exportSchema = false              // skip writing schema JSON
)
abstract class AppDatabase : RoomDatabase() { // abstract = Room generates the impl
    abstract fun ticketDao(): TicketDao       // exposes the DAO, Room writes the body

    companion object {
        @Volatile                             // visible to all threads immediately
        private var instance: AppDatabase? = null // starts null, built on first call

        fun getInstance(context: Context): AppDatabase {
            return instance ?: synchronized(this) {  // if not null, skip the lock
                // second check: another thread may have built it while we waited
                instance ?: Room.databaseBuilder(
                    context.applicationContext,   // app context, not Activity (no leak)
                    AppDatabase::class.java,      // the class Room will implement
                    "memory_ticket.db"            // the SQLite file name on disk
                ).build()
                 .also { instance = it }          // store it, then return it
            }
        }
    }
}
```

---

###### Line-by-line: the singleton explained

```kotlin
return instance ?: synchronized(this) {
```
- `instance ?:` -> "if instance is not null, return it right now — done"
- `synchronized(this)` -> if it WAS null, lock this block so only one thread enters

```kotlin
    instance ?: Room.databaseBuilder(...).build().also { instance = it }
```
- second `instance ?:` -> check AGAIN inside the lock (another thread may have just built it)
- `Room.databaseBuilder(...)` -> constructs the DB
- `.build()` -> finalises construction
- `.also { instance = it }` -> stores the new DB in `instance` BEFORE returning it

---

###### ASCII Flowchart

```text
getInstance(context) called
        |
        v
  instance != null? ---YES---> return instance (fast path)
        |
       NO
        |
        v
  synchronized(this) -- lock acquired
        |
        v
  instance != null? ---YES---> return instance (another thread just built it)
        |
       NO
        |
        v
  Room.databaseBuilder(...).build()
        |
        v
  .also { instance = it }  -- store it
        |
        v
  return the new instance
```

---

###### Common Mistakes
- BAD: `@Database([TicketEntity::class], ...)` -> missing `entities =` label, won't compile
- BAD: `context` instead of `context.applicationContext` -> DB holds Activity reference -> memory leak
- BAD: no `synchronized` -> two threads can build two databases at the same time
- BAD: only one `instance ?:` check -> thread A waits, thread B builds, thread A builds again

---

###### Weak Area Clarification
- Why check `instance ?:` twice (outside AND inside `synchronized`)?
- Outside -> fast path, skips the expensive lock when DB already exists
- Inside -> safety net, another thread may have built it while this thread was waiting for the lock
- Both checks together = the "double-checked locking" pattern

---

###### Memory Hook
- One cabinet, one lock, check twice before building.

---

###### Key Rule
- Always pass `context.applicationContext` to `databaseBuilder` — never an Activity context.

---

###### Related
- [[00 - Room - Overview]]
- [[02 - Room - DAO]]
- [[04 - Room - Repository]]
