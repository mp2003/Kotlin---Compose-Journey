# Insights — Learning notes from the 1Pharmacy retailer app

Per-feature, per-file notes that explain **what practice each file demonstrates, why the
codebase does it that way, and where the rule comes from**. Read these alongside the real
code (repo: `~/AndroidStudioProjects/KMM`) to learn the production conventions.

> Authoritative spec in the repo: `1PRetailer/ARCHITECTURE.md`. These notes summarise it and
> point into the real files; when in doubt, ARCHITECTURE.md wins.

---

## App-wide practices to follow (the rules these features obey)

Every `android-retailer-*` feature module follows these. Each insight file shows one in action.

1. **Clean Architecture layers** — `presentation/ → domain/ ← data/`.
   `domain/` is framework-free (no Android, Retrofit, Room imports). Dependencies point
   *inward*: data and presentation depend on domain, never the reverse.

2. **MVI, unidirectional** — `Action → ViewModel.onAction → UiState (.copy) → UI`.
   `UiState` is an immutable `data class`. The UI only sends Actions and renders State.

3. **Business logic lives in UseCases, not ViewModels.** The ViewModel validates input,
   copies state, and fires Effects — nothing more.

4. **`Result<T, E>` is mandatory** on Repository + UseCase methods.
   It is `com.one.pharma.core.result.Result` (from `:android-core`) — **not** `kotlin.Result`.
   No throwing exceptions for control flow.

5. **`DataError → UseCaseError` happens in the UseCase.** Repos return `DataError`;
   UseCases convert it to a user-facing `UseCaseError` with a stable id from `[Feature]ErrorIds`.

6. **`InitialState` ≠ `isLoading`.** `InitialState` (sealed: Loading/Success/Error) is for the
   one-time screen-load fetch. `isLoading` (a flag on UiState) is for user-triggered actions.

7. **Effect vs Navigation are two separate sealed hierarchies.** `Effect` (ViewModel→RootScreen)
   carries *all* one-shot events (toasts, nav). `Navigation` (RootScreen→Destination) carries
   *only* navigatable commands. Don't collapse them.

8. **RootScreen / Screen split.** `[Feature]RootScreen` is stateful (takes the ViewModel,
   collects Effect, drives InitialState, forwards navigation). `[Feature]Screen` is pure —
   takes `state` + callbacks, so it is preview/test friendly.

9. **DI source selection lives in `di/` via RemoteConfig.** The module picks Mock vs Real
   provider/cache. The Repository must not know which it got.

10. **`dto/` vs `entity/` is a hard split.** `dto` = flat network shape (`@SerializedName`);
    `entity` = Room (`@Entity`). The `mapper/` bridges them; never reuse one as the other.

11. **No magic strings.** Error ids → `[Feature]ErrorIds`; RemoteConfig keys → constants;
    UI text → string resources.

12. **Convention plugins own the boilerplate.** A feature's `build.gradle.kts` is just a
    `plugins {}` block + module-specific deps. SDK/Java/Compose config lives in `build-logic/`.

---

## Features

- [[store-profile/README|store-profile]] — view-only screen; my first feature scaffold.
  Simplest possible flow (load once → render), so it shows the full layer skeleton cleanly.
