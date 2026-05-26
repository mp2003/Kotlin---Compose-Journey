###### How I Learn Best

- See a small piece of real code -> understand every line -> type it myself -> build up gradually
- NOT: fill-in-the-blank skeletons with TODOs -> feels abstract, no confidence
- NOT: steps + syntax names only (no code) -> too vague for brand-new syntax
- NOT: full files dumped at once -> overwhelming
- Works: "tiny pieces, one at a time, each piece shown + explained, I retype it"
- Works: after seeing a pattern once, less help next time (taper assistance)

---

###### Strengths (what clicks fast)

- MVI pattern (UiState / Event / Effect / ViewModel) -> very solid, built multiple screens
- StateFlow + Flow -> understands push model, collectAsState, update{}
- Repository pattern -> understood it in networking (Week 3), applied it to Room immediately
- Constructor injection -> understood from Hilt (Week 2), applied without prompting
- Kotlin syntax basics -> data class, sealed class, when(), nullable types, companion object
- Self-correction -> when shown the specific line to fix, fixes it fast and correctly
- First-attempt accuracy -> Entity, DAO, Repository all correct on first try once pattern was understood

---

###### Struggles & Weak Areas

- Brand-new syntax with no prior exposure
  - Cannot write it from steps + names alone
  - Needs to SEE it first, piece by piece, explained line by line
  - Example: AppDatabase singleton pattern (double-checked locking, @Volatile, synchronized)

- Abstract concepts before seeing any code
  - Prose explanations without code feel disconnected
  - Always needs code to anchor the concept

- Confidence on unfamiliar ground
  - Knows more than he thinks — tends to freeze before trying
  - Once shown the first piece, writes the rest quickly

- Cross-referencing notes while coding
  - Notices when a concept is missing from notes and calls it out
  - Notes need to exist BEFORE the coding step, not after

---

###### Teaching Style That Works

- For brand-new syntax: show piece by piece, explained, user retypes each piece
- For familiar patterns: give the shape/steps, let the user write it, review after
- For concepts: always pair with a code example — no pure prose explanations
- Taper help over time: first exposure = full, second time = hints, third = user flies solo
- Keep notes in sync: write the vault note BEFORE giving the coding step

---

###### Weeks Completed (as of 2026-05-26)
- Week 1: MVI Architecture -> complete, strong
- Week 2: Hilt DI -> complete, strong
- Week 3: Networking (Retrofit) -> complete, strong
- Week 4: Navigation + Compose UI -> in progress (NavHost, args, nested graphs, OTP flow, Profile, Todo done; bottom nav + animations remaining)
- Week 5: Room -> in progress (data layer complete: Entity, Dao, Database, Repository)
- Week 6: CameraX -> not started
