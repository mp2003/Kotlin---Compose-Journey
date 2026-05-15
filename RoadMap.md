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
- [ ] Nested graphs
- [ ] Bottom navigation

## Coding
- [x] Setup navigation routes (sealed `ScreenState` route definitions)
- [x] Pass arguments (typed `navArgument` + `NavType.StringType` / `NavType.IntType`, default values)
- [ ] Back navigation

### UI Patterns
- [ ] LazyColumn / LazyRow
- [ ] Scaffold
- [ ] BottomSheet / Dialog
- [ ] Animations
- [ ] Coil image loading

### Build
- [ ] Extend Posts App with navigation

**Project:** `NavigationApp` — `NavigationMvi` (NavHost), `ScreenState` sealed routes, `DetailScreen` receiving `name`/`age` args, MVI layer (`MainViewModel`, `MainUiState/Event/Effect`) carried over from Week 1.

**Notes:** built a 2-screen graph (MainScreen → DetailScreen), passed String + Int arguments through the route with `navArgument`, used default & nullable argument config, integrated navigation with the existing MVI screen.

---

# WEEK 5 — Room + DataStore

## Theory
- [ ] Room basics
- [ ] DataStore usage
- [ ] Flow from DB
- [ ] Offline-first pattern

## Coding
- [ ] Create Entity
- [ ] DAO queries
- [ ] Database setup

### DataStore
- [ ] Store token
- [ ] Store preferences

### Build
- [ ] Offline-enabled Posts App

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
| Week 5 | ⬜ Not started |
| Week 6 | ⬜ Not started |
| Week 7 | ⬜ Not started |
| Week 8 | ⬜ Not started |

---

# Current Focus

- Week 4 — Navigation + Compose UI (in progress)
    - ✅ Done: NavHost/NavController, route setup, typed argument passing (NavigationApp)
    - Next: back navigation, nested graphs, bottom navigation
    - Then: UI patterns (LazyColumn, Scaffold, Coil) + extend Posts App

---

# June Goal

> Become production-ready Kotlin Android developer
