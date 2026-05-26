# Kotlin Android — Complete Revised

## Roadmap

- **Duration:** 8 Weeks → June 2026
- **Starting Point:** Advanced UI State Modeling
- **Schedule:** Evenings + Weekends (~10–12 hrs/week)
- **Style:** 80% Coding / 20% Theory
- **Goal:** Read, understand, and contribute to any Kotlin project

---

# Completed Chapters

- [x] Ch 1: Kotlin Language Fundamentals
- [x] Ch 2: Coroutines (launch, async, suspend, scopes)
- [x] Ch 3: Flow & StateFlow
- [x] Ch 4: Flow Operators
- [x] Ch 5: Compose Basics
- [x] Ch 6: Compose State
- [x] Ch 7: State Hoisting
- [x] Ch 8: Unidirectional Data Flow
- [x] Ch 9: ViewModel + StateFlow
- [x] Ch 10: Async Data Handling

---

# WEEK 1 — UI State Modeling (MVI)  ✅ COMPLETE

## Theory
- [x] What is MVI (Model-View-Intent)
- [x] MVI vs MVVM
- [x] Single state object
- [x] Sealed class vs sealed interface
- [x] Immutable state (.copy())

## Coding

### Exercise 1 — UI State
- [x] Create `ScreenUiState`
- [x] Handle Loading / Success / Error / Empty
- [x] Render UI using `when(state)`

### Exercise 2 — Intents
- [x] Create `ScreenIntent`
- [x] Implement `onIntent(intent)`
- [x] Update `StateFlow<UiState>`

### Exercise 3 — Effects
- [x] Create `UiEffect`
- [x] Use `SharedFlow`
- [x] Handle in Compose with `LaunchedEffect`

### Build
- [x] Task Manager Screen (full MVI flow)

**Notes written:** MVI - Overview, State, Intent, Effect, ViewModel, onIntent, Reducer, Task Manager Exercise, Task Manager Full Code

---

# WEEK 2 — Dependency Injection (Hilt)  ✅ COMPLETE

## Theory
- [x] What is DI
- [x] Injection (constructor injection)
- [x] @Provides vs @Binds (side-by-side comparison)
- [x] Dependency Inversion (the principle, not just DI)
- [x] Scopes (Singleton, ViewModel, Activity, Fragment)

## Coding

### Setup
- [x] Add Hilt dependencies
- [x] Create `@HiltAndroidApp`
- [x] Annotate Activity

### Injection
- [x] Create Repository interface
- [x] Implement with `@Inject`
- [x] Bind using `@Module`

### Third-party
- [x] Provide Retrofit / OkHttp (concept covered with FakeApiService via `@Provides`)
- [x] Use qualifiers (`@Named`, `@Qualifier`)

### Build
- [x] Refactor Task Manager with Hilt

**Notes written:** DI - Overview, Injection - Constructor Injection, Hilt - DI Library, Hilt - Architecture Layers, Hilt - Runtime Flow, Hilt - Third-party with @Provides, Dependency Inversion, Qualifiers, Named, Task Manager - Hilt Refactor, Hilt - Provides vs Binds, Hilt - Scopes

---

# WEEK 3 — Networking (Retrofit + OkHttp)  ✅ COMPLETE

## Theory
- [x] REST APIs in Android
- [x] Retrofit vs OkHttp
- [x] Serialization
- [x] Interceptors

## Build — Posts Feed App (phase by phase)

### Phase 1 — Basic GET 
- [x] Add Retrofit + OkHttp dependencies
- [x] Create `Post` data class
- [x] Create `PostsApiService` interface with GET
- [x] Build Retrofit instance manually
- [x] Make the call, log the result

### Phase 2 — Introduce Repository
- [x] Create `PostRepository`
- [x] Move API call into repository
- [x] ViewModel calls repository

### Phase 3 — MVI Integration
- [x] Create `PostsUiState`
- [x] Create `PostsIntent`
- [x] Wire ViewModel → StateFlow → UI

### Phase 4 — Error Handling
- [x] Create `ApiResult` sealed class
- [x] Map Loading / Success / Error to UI state

### Phase 5 — Interceptors
- [x] Add logging interceptor
- [x] Add auth header interceptor
- [x] Understand token refresh (concept only)

## Progress Tracker
| Phase                    | Status     |
| ------------------------ | ---------- |
| Phase 1 — Basic GET      | ✅ Complete |
| Phase 2 — Repository     | ✅ Complete |
| Phase 3 — MVI            | ✅ Complete |
| Phase 4 — Error Handling | ✅ Complete |
| Phase 5 — Interceptors   | ✅ Complete |

**Notes written:** Phase 1 - Basic GET Call, Phase 2 - Repository, Phase 3 - MVI, Phase 4 - Error Handling, Phase 5 - Interceptors

## Notes
- Phase 1: learned how REST API calls work in Android using Retrofit, OkHttp, and a simple GET request.
- Phase 2: moved the network call into `PostRepository` so data access stayed outside the UI layer.
- Phase 3: connected API data to the screen with MVI using `PostUiState`, `PostIntent`, `PostViewModel`, and `StateFlow`.
- Phase 4: introduced `ApiResult` so success and error handling became structured instead of raw.
- Phase 5: added a logging interceptor and an auth header interceptor to understand request flow and headers.

---

# WEEK 4 — Navigation + Compose UI  🔶 IN PROGRESS

## Theory
- [x] NavHost & NavController
- [x] Arguments passing
- [x] Nested graphs
- [ ] Bottom navigation

## Coding
- [x] Setup navigation routes (sealed `ScreenState` route definitions)
- [x] Pass arguments (typed `navArgument` + `NavType.StringType` / `NavType.IntType`, default values)
- [x] Back navigation (`popBackStack()` hoisted as `onBack: () -> Unit` — screen decoupled from NavController)
- [x] Nested graph — `navigation(route = "auth", startDestination = "login")` as a `NavGraphBuilder.authGraph()` extension; cleared whole flow with `popUpTo(AuthGraph) { inclusive = true }`

### UI Patterns
- [x] LazyColumn / LazyRow (`LazyColumn` + `items(key = { it.id })` in Todo list; stable keys for correct add/delete)
- [x] Scaffold (Profile screen `Scaffold(snackbarHost = ...)` with `innerPadding`)
- [x] Dialog (`DatePickerDialog` on MainScreen DOB picker)
- [ ] BottomSheet
- [ ] Animations (touched `basicMarquee` on Profile title)
- [ ] Coil image loading

### Build
- [ ] Extend Posts App with navigation
- [x] OTP Login flow (username → OTP → verify) with full MVI + fake authenticator + resend countdown timer
- [x] Profile screen (View/Edit modes, draft pattern for true Cancel, snackbar effect, nav-arg userName)
- [x] Todo list screen (add / toggle / delete with immutable list ops, derived counter, empty state)

**Project:** `NavigationApp` — `NavigationMvi` (NavHost), `ScreenState` sealed routes, `DetailScreen` receiving `name`/`age` args, MVI layer (`MainViewModel`, `MainUiState/Event/Effect`) carried over from Week 1.

**Notes:** built a 2-screen graph (MainScreen → DetailScreen), passed String + Int arguments through the route with `navArgument`, used default & nullable argument config, integrated navigation with the existing MVI screen. Added back navigation via hoisted `onBack` lambda (DetailScreen stays decoupled from NavController — preview/test friendly). Added numeric age input: `KeyboardType.Number` keyboard + digit-only validation enforced in the ViewModel reducer (UX hint vs. actual enforcement). Built a nested auth graph (`navigation/AuthGraph.kt`): Login → Register → MainScreen, with the auth flow scoped under route `"auth"` and cleared via `popUpTo(... ) { inclusive = true }` so Back from Main exits instead of re-entering auth. Learned: a graph has its own route distinct from its screens; `NavGraphBuilder` extension functions keep `NavHost` clean and make each feature flow a self-contained, scoped unit.

**Session additions (MVI deep-dive + new screens):**
- **OTP Login flow** — replaced placeholder Login with full MVI: `LoginUiState` (username/otp/step enum/loading/error/resendSecondsLeft), `LoginUiEvent`, `LoginUiEffect` (NavigateToMain, ShowMessage). `OtpAuthenticator` interface + `FakeOtpAuthenticator` (hardcoded OTP "1234") behind an interface so a real backend swaps in one file. Built a **resend countdown** with `viewModelScope` + `delay` + a cancellable `Job` (cancel old timer before starting new — fixes spam-tap). On login success, passes `username` out via `onLoginSuccess(state.username)` callback.
- **Pass data through nav routes** — login username threaded Login → `main_screen/{userName}` → DetailScreen. Learned the route-template contract: `{placeholder}` literal in route + matching `navArgument(type)` + matching `getXxx(key)` read; same name/order/count in all places or it crashes (`destination cannot be found`). `NavType.LongType` (not Int) for epoch millis.
- **DatePicker** — replaced age text input with Material3 `DatePickerDialog`/`rememberDatePickerState` (DOB as epoch millis Long); `calculateAge`/`formatDob` extracted as **pure top-level functions** (domain layer, not ViewModel — logic that's stateless belongs in utils, not VM).
- **Profile screen (solo build)** — View/Edit mode enum; **draft pattern** (`name` committed vs `draft` in-flight) so Cancel truly discards; Save promotes draft→name + fires snackbar; `Scaffold` + `SnackbarHostState` + `LaunchedEffect` effect collector; Save button `enabled = draft != name` (derived display decision).
- **Todo list (solo build)** — list state (`List<TodoItem>`), **id-carrying events** (`ToggleTodo(id)`, `DeleteTodo(id)`), immutable list updates (`+`, `.map`, `.filter` — never mutate), `LazyColumn` with stable keys, reusable `TodoRow` taking callbacks, derived "N of M done" counter, empty state.

**Concepts solidified:** State (`.update`, sticky) vs Effect (`Channel.send`, one-shot, consumed once); why `collectAsState` subscribes (push not poll) vs `remember { mutableStateOf }` (local snapshot, no updates) — debugged a real "Edit not updating" bug from this; `viewModel()` helper vs `ViewModel()` constructor (latter re-runs on recompose → state resets → debugged a "buggy input" bug from this); where logic lives (stateful → ViewModel, pure → domain utils, render decision → composable); a screen with no logic (DetailScreen) needs no ViewModel.

**Notes written:** Navigation - Overview, NavHost and NavController, Arguments Passing, Back Navigation, Numeric TextField Input, Nested Graphs, OTP Login MVI, Resend Countdown Timer, Route Argument Contract, DatePicker + DOB, State vs Effect, collectAsState vs mutableStateOf, viewModel() vs constructor, Draft Pattern, List State + id Events, LazyColumn, Scaffold + Snackbar

---

# WEEK 5 — Room + DataStore  🔶 IN PROGRESS

## Theory
- [x] Room basics
- [ ] DataStore usage
- [x] Flow from DB
- [x] Offline-first pattern

## Coding
- [x] Create Entity (`TicketEntity` — @Entity, @PrimaryKey, date as Long, photo as Uri String)
- [x] DAO queries (`TicketDao` — observeAll Flow, observeById Flow?, insert suspend Long, deleteById suspend)
- [x] Database setup (`AppDatabase` — @Database, abstract class, singleton companion object)
- [x] Repository (`TicketRepository` — thin wrapper, constructor injection)
- [x] ServiceLocator — manual DI object

### DataStore
- [ ] Store token
- [ ] Store preferences

### Build
- [ ] Offline-enabled Posts App
- [x] Memory Ticket App — data layer (via MemoryTicket project)

**Notes written:** Room - Overview, Entity, DAO, Database, Repository, Flow from Database, ServiceLocator (manual DI)
**Project:** `MemoryTicket` — `/Users/mahi/AndroidStudioProjects/MemoryTicket`

---

# WEEK 6 — Permissions + Camera + Files

## Theory
- [ ] Runtime permissions
- [ ] CameraX
- [ ] Intents
- [ ] File handling

## Coding
- [ ] Request permissions
- [ ] Camera preview
- [ ] Capture photo
- [ ] QR scanning

### Files
- [ ] Pick image/file
- [ ] Upload via Retrofit

### Build
- [ ] Scanner App

---

# WEEK 7 — Hardware Integration

## Theory
- [ ] Bluetooth basics
- [ ] Printer SDK pattern
- [ ] BroadcastReceiver
- [ ] Services
- [ ] Firebase FCM
- [ ] Deep Links

## Coding
- [ ] Scan Bluetooth devices
- [ ] Connect & send data
- [ ] Integrate SDK

### Firebase
- [ ] Setup FCM
- [ ] Handle notifications

### Build
- [ ] Device Integration App

---

# WEEK 8 — Full App + Real Code

## Theory
- [ ] Gradle basics
- [ ] Build variants
- [ ] Reading real projects
- [ ] Debugging tools

## Coding

### Full App
- [ ] Inventory Manager App

### Open Source
- [ ] Read 2 projects
- [ ] Trace architecture
- [ ] Fix a bug / add feature

### Gradle
- [ ] Add dependency
- [ ] Create build variant
- [ ] Generate APK

---

# FINAL OUTCOME

- [ ] Understand any Kotlin project
- [ ] Trace full data flow
- [ ] Work with Hilt, Retrofit, Room
- [ ] Handle real-world features
- [ ] Contribute to production apps

---

# Progress Tracker

| Week   | Status        |
| ------ | ------------- |
| Week 1 | ✅ Complete    |
| Week 2 | ✅ Complete |
| Week 3 | ✅ Complete |
| Week 4 | 🔶 In progress |
| Week 5 | 🔶 In progress |
| Week 6 | ⬜ Not started |
| Week 7 | ⬜ Not started |
| Week 8 | ⬜ Not started |

---

# Current Focus

- Week 4 — Navigation + Compose UI (in progress)
    - ✅ Done: NavHost/NavController, route setup, typed argument passing, back navigation, nested graphs, OTP login flow, DatePicker dialog, Profile screen (draft pattern), Todo list (LazyColumn), Scaffold + Snackbar (NavigationApp)
    - Remaining: bottom nav, BottomSheet, animations, Coil image loading
    - Being practiced via: MemoryTicket App (Sessions 2–3)

- Week 5 — Room + DataStore (in progress via MemoryTicket App)
    - ✅ Done: Entity, DAO, Database, Repository, Flow-from-DB
    - Next: ServiceLocator (manual DI), then wire to UI in Session 2
    - Project: `/Users/mahi/AndroidStudioProjects/MemoryTicket`

---

# June Goal

> Become production-ready Kotlin Android developer
