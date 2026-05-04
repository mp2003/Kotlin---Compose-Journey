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

# WEEK 2 — Dependency Injection (Hilt)  🟡 IN PROGRESS

## Theory
- [x] What is DI
- [x] Injection (constructor injection)
- [x] @Provides vs @Binds (side-by-side comparison)
- [ ] Dependency Inversion (the principle, not just DI)
- [ ] Scopes (Singleton, ViewModel, Activity, Fragment)

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
- [ ] Refactor Task Manager with Hilt

**Notes written:** DI - Overview, Injection - Constructor Injection, Hilt - DI Library, Hilt - Architecture Layers, Hilt - Runtime Flow, Hilt - Third-party with @Provides

**Still to do (theory):**
- `Hilt - @Provides vs @Binds` note (side-by-side: when each one applies)
- `Hilt - Dependency Inversion` note (the SOLID principle, why interfaces matter)
- `Hilt - Scopes` note (Singleton, ViewModel, Activity, Fragment scopes)
- `Hilt - Qualifiers` note (`@Named`, custom `@Qualifier`)

**Still to do (build):**
- Refactor Task Manager screen with Hilt (inject ViewModel + Repository)

---

# WEEK 3 — Networking (Retrofit + OkHttp)

## Theory
- [ ] REST APIs in Android
- [ ] Retrofit vs OkHttp
- [ ] Serialization
- [ ] Interceptors

## Coding

### Retrofit
- [ ] Create API interface
- [ ] Use GET / POST / DELETE
- [ ] Add query params

### Interceptors
- [ ] Logging
- [ ] Auth header
- [ ] Token refresh

### Error Handling
- [ ] Create `ApiResult`
- [ ] Map to UI state

### Build
- [ ] Posts App (API integration)

---

# WEEK 4 — Navigation + Compose UI

## Theory
- [ ] NavHost & NavController
- [ ] Arguments passing
- [ ] Nested graphs
- [ ] Bottom navigation

## Coding
- [ ] Setup navigation routes
- [ ] Pass arguments
- [ ] Back navigation

### UI Patterns
- [ ] LazyColumn / LazyRow
- [ ] Scaffold
- [ ] BottomSheet / Dialog
- [ ] Animations
- [ ] Coil image loading

### Build
- [ ] Extend Posts App with navigation

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
| Week 2 | 🟡 In progress |
| Week 3 | ⬜ Not started |
| Week 4 | ⬜ Not started |
| Week 5 | ⬜ Not started |
| Week 6 | ⬜ Not started |
| Week 7 | ⬜ Not started |
| Week 8 | ⬜ Not started |

---

# Current Focus

- Finish Week 2 theory: write 4 missing notes
    1. `Hilt - @Provides vs @Binds` (side-by-side comparison)
    2. `Hilt - Dependency Inversion` (the principle behind DI)
    3. `Hilt - Scopes` (Singleton, ViewModel, Activity, Fragment)
    4. `Hilt - Qualifiers` (`@Named` and custom `@Qualifier`)
- Then build: refactor Task Manager screen with Hilt

---

# June Goal

> Become production-ready Kotlin Android developer
