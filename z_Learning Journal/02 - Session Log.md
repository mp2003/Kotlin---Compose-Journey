###### How to Use
- Add a new entry after each coding session
- Be honest — what clicked, what didn't, what to revisit
- Link to the relevant notes so revision is one tap away

---

###### Session 1 — 2026-05-25 / 26 — Room Data Layer (Memory Ticket App)

**What was built:**
- `TicketEntity` — @Entity, @PrimaryKey, 6 fields
- `TicketDao` — 4 operations (observeAll, observeById, insert, deleteById)
- `AppDatabase` — @Database annotation, abstract class, singleton companion object
- `TicketRepository` — thin wrapper over DAO, constructor injection
- `ServiceLocator` — manual DI object (to write next)

**What clicked:**
- Entity fields — correct types (Long for date, String for Uri) on first attempt
- DAO — Flow for reads / suspend for writes — applied correctly
- Repository — thin delegation, familiar from Week 3 networking

**What didn't click / needed extra help:**
- AppDatabase singleton pattern — needed piece-by-piece breakdown
  - specifically: double-checked locking, @Volatile, synchronized(this), .also{}
  - couldn't start from steps alone — needed to see the code first
- Missing `entities =` label in @Database annotation — caught in review
- Missing package declaration in TicketDao — caught in review
- `observeById` return type not nullable initially — caught in review
- Typo: `observerById` instead of `observeById` — caught in review

**Weak areas flagged this session:**
- Singleton pattern (double-checked locking) -> see [[03 - Weak Areas & Revision Topics]]
- @Database annotation parameters -> see [[03 - Weak Areas & Revision Topics]]

**Notes written:**
- [[Room/00 - Room - Overview]]
- [[Room/01 - Room - Entity]]
- [[Room/02 - Room - DAO]]
- [[Room/03 - Room - Database]]
- [[Room/04 - Room - Repository]]
- [[Room/05 - Room - Flow from Database]]
- [[Room/06 - Room - ServiceLocator (manual DI)]]

---

###### Session 2 — 2026-05-27 — List Screen + MVI + App Launch

**What was built:**
- `ListUiState` / `ListEvent` — sealed interfaces
- `ListViewModel` — Room Flow -> StateFlow via `.stateIn()`
- `ListViewModelFactory` — `ViewModelProvider.Factory` for constructor injection
- `TicketListScreen` — Scaffold, TopAppBar, FAB, Loading/Empty/Success states, LazyColumn
- `ScreenState` — sealed class for typed nav routes
- `MemoryTicketNavHost` — NavHost wired to list screen
- `MainActivity` — app entry point
- App ran successfully — empty state visible

**What clicked:**
- `.stateIn()` pattern — understood and applied correctly
- `LazyColumn` with `items(key = { it.id })` — familiar from Week 4
- `Scaffold` + `TopAppBar` + `FAB` — applied correctly
- `when(state)` with `is` on data class, plain on objects — correct

**What didn't click / needed help:**
- `ViewModelProvider.Factory` — new concept, needed full explanation
- Missing `package` declaration on multiple files — recurring mistake to watch
- `NavController` type annotation — should let Kotlin infer it
- `onCreate` override — picked wrong two-parameter version
- Extra `{ }` wrapping inside Scaffold content block

**Recurring pattern to fix:**
- Always write `package com.milind.memoryticket.xxx` as the FIRST line of every new file
