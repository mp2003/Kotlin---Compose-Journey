###### Elevator Pitch
- Room is Android's database library that turns Kotlin classes into SQLite tables using a few annotations, so you store data on the device without writing raw SQL.

---

###### Definition
- Room is a persistence library that maps `@Entity` classes to database tables and `@Dao` interfaces to SQL queries.

---

###### Real-World Analogy
- `@Entity` -> the blank form (shape of one row)
- `@Dao` -> the clerk who files and fetches forms
- `@Database` -> the filing cabinet holding it all
- Flow query -> a bell that rings every time a form changes

---

###### What
- Three pieces -> [[01 - Room - Entity]], [[02 - Room - DAO]], [[03 - Room - Database]]
- [[04 - Room - Repository]] -> wraps the DAO so the UI never touches Room directly
- [[05 - Room - Flow from Database]] -> queries that auto-update the UI
- [[06 - Room - ServiceLocator (manual DI)]] -> hand the repository out without Hilt

---

###### Why
- Saves data so it survives app restarts (persistence)
- Type-safe -> SQL mistakes caught at compile time, not at runtime
- Returns `Flow` -> UI auto-refreshes when data changes (offline-first)

---

###### Core Concepts
- Entity -> a class that equals one table
- DAO -> Data Access Object, the SQL functions
- Database -> the abstract class that glues entities and DAOs together
- KSP -> the compiler that generates Room's real code from the annotations

---

###### How it Works
- Annotate a data class with `@Entity` -> Room makes a table
- Define a `@Dao` interface with `@Insert` / `@Query` -> Room writes the SQL impl
- Build the `@Database` once as a singleton
- A repository calls the DAO; the ViewModel calls the repository

---

###### Syntax
- `@Entity` -> marks a class as a table
- `@Dao` -> marks an interface as the query set
- `@Database` -> marks the abstract DB class
- `Room.databaseBuilder(...)` -> builds the database instance

```kotlin
// The three annotations, at a glance
@Entity data class Ticket(...)        // table
@Dao interface TicketDao { ... }      // queries
@Database(entities = [Ticket::class], version = 1)
abstract class AppDatabase : RoomDatabase() { abstract fun dao(): TicketDao }
```

---

###### Layered Diagram

```text
+----------------------------+
|  UI (Compose)              |
+----------------------------+
|  ViewModel (MVI state)     |
+----------------------------+
|  Repository                |
+----------------------------+
|  DAO  (@Query / @Insert)   |
+----------------------------+
|  Database (SQLite file)    |
+----------------------------+
```

---

###### Memory Hook
- Entity = table, DAO = clerk, Database = cabinet.

---

###### Key Rule
- The UI never touches Room directly -> always go through a repository.

---

###### Related
- [[01 - Room - Entity]]
- [[02 - Room - DAO]]
- [[03 - Room - Database]]
- [[04 - Room - Repository]]
- [[05 - Room - Flow from Database]]
- [[06 - Room - ServiceLocator (manual DI)]]
