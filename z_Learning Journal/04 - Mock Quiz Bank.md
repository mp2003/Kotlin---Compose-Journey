###### How to Use
- Ask Claude: "quiz me on Room" or "give me questions from the quiz bank"
- Try answering without looking at notes first
- Check your answer, then mark how it went: OK / shaky / missed

---

###### Room — Entity

- Q: What annotation marks a class as a database table?
  A: @Entity

- Q: What does `@PrimaryKey(autoGenerate = true)` do?
  A: Room auto-assigns a unique id to each new row; you pass `id = 0` to signal a new row

- Q: Why store date as `Long` and not a formatted `String`?
  A: Long (epoch millis) lets you sort, compare, and calculate. A formatted string can't be sorted reliably.

- Q: Why store a photo as a `String` Uri and not image bytes?
  A: Keeps the DB row tiny; the file lives on disk and the DB just holds the path to it

---

###### Room — DAO

- Q: What annotation marks an interface as the set of database operations?
  A: @Dao

- Q: When does a DAO function use `suspend`?
  A: For writes (INSERT, UPDATE, DELETE) — one-shot operations that run off the main thread

- Q: When does a DAO function return `Flow`?
  A: For reads — so the result stays live and re-emits every time the table changes

- Q: What does `:id` inside a @Query SQL string do?
  A: Binds the Kotlin function parameter named `id` into the SQL query

- Q: Why should `observeById` return `Flow<TicketEntity?>` and not `Flow<TicketEntity>`?
  A: The row might not exist (deleted or wrong id) — nullable forces you to handle that case

---

###### Room — Database

- Q: Why is `AppDatabase` an abstract class and not a regular class?
  A: Room generates the real implementation via KSP — you only describe it; Room writes the body

- Q: What does `@Volatile` on the instance field do?
  A: Makes the field's value immediately visible to all threads — prevents one thread seeing a stale null

- Q: Why check `instance ?:` twice in getInstance()?
  A: First check skips the lock if already built (fast path). Second check (inside synchronized) handles the case where another thread built it while this thread was waiting.

- Q: Why use `context.applicationContext` and not just `context`?
  A: The database outlives any Activity — holding an Activity context would cause a memory leak

- Q: What is missing from `@Database([TicketEntity::class], version = 1)`?
  A: The `entities =` label — parameters in @Database are named, not positional

---

###### Room — Repository

- Q: What is the purpose of the repository?
  A: It wraps the DAO so the ViewModel never touches Room directly — one seam for all data access

- Q: Should repository functions that return Flow be `suspend`?
  A: No — they just return the Flow from the DAO. Only write operations are suspend.

- Q: Where does the DAO come from in a repository?
  A: Constructor injection — passed in when the repository is created, not built inside it
