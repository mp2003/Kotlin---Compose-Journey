---
up: "[[00 - Room - Overview]]"
---

###### Elevator Pitch
- An Entity is a Kotlin data class marked with `@Entity` so Room treats it as one table, with each property becoming a column.

---

###### Definition
- An `@Entity` is a class that defines the shape of a single row in a database table.

---

###### Real-World Analogy
- Entity -> a blank form template
- Each property -> a field on the form (name, date, photo)
- One saved row -> one filled-in form
- Primary key -> the form's unique serial number

---

###### What
- `@Entity(tableName = "...")` -> declares the table and names it
- `@PrimaryKey` -> the unique id column
- Each `val` -> one column, the Kotlin type maps to a SQL type

---

###### Why
- Defines exactly what one record looks like
- Room reads it to create the table at compile time
- Type-safe -> wrong column names fail to compile

---

###### Core Concepts
- Primary key -> the unique id for each row
- `autoGenerate = true` -> Room assigns the id, you pass 0
- Type mapping -> `Long`/`String`/`Int`/`Boolean` map to SQLite columns automatically

---

###### How it Works
- You write a `data class` with the fields you want to store
- Add `@Entity` on the class and `@PrimaryKey` on the id
- KSP generates the `CREATE TABLE` SQL behind the scenes
- Inserting an object with id `0` lets Room fill in the real id

---

###### Syntax
- `@Entity(tableName = "tickets")` -> this class is the `tickets` table
- `@PrimaryKey(autoGenerate = true)` -> id column, auto-assigned
- `val id: Long = 0` -> 0 means "no id yet, Room please assign"

```kotlin
// @Entity = this class is a table
@Entity(tableName = "tickets")
data class TicketEntity(
    // Auto-assigned unique id; 0 means "new row"
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val title: String,        // text column
    val location: String,     // text column
    val dateMillis: Long,     // store dates as epoch millis, not strings
    val note: String,         // text column
    val photoUri: String      // store the file Uri string, not the image bytes
)
```

---

###### Common Mistakes
- BAD: storing a date as a formatted `String` (can't sort/compare) -> use `Long` millis
- BAD: storing the image bytes in the DB -> store the file Uri string instead
- BAD: giving the id no default -> use `= 0` so autoGenerate works on insert

---

###### Weak Area Clarification
- Why `id: Long = 0`? Room reads 0 as "unset", then assigns the next real id on insert.
- The `0` is never actually stored as a real row id.

---

###### Memory Hook
- One Entity = one table = one form template.

---

###### Key Rule
- Store dates as `Long` and photos as a Uri `String` -> keep rows small and sortable.

---

###### Related
- [[00 - Room - Overview]]
- [[02 - Room - DAO]]
