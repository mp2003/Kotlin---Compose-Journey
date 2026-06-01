# store-profile — feature insights

A **view-only** screen: load store details once, render them. The simplest end-to-end
flow, so it shows the whole layer skeleton without distractions.

**Code lives in:** `~/AndroidStudioProjects/KMM/features/android-retailer-store-profile-impl/`
**Package:** `io.one.pharma.retailer.store.profile.impl`

## Flow (what happens when you open it)

```
Drawer tap
  → navigate to StoreProfileRoutes.Graph
    → StoreProfileRootScreen (stateful)
       → ViewModel.init → UseCase.getStoreProfile()
          → Repository.getStoreProfile()  → SettingsRepository.getStoreSettings()  [existing source]
       → Result<StoreProfile, _>
       → InitialState = Success → StoreProfileScreen (pure) renders state
```

## File tree → insight map

```
domain/
  model/StoreProfile.kt ........... [[domain/model-store-profile]]
  StoreProfileRepository.kt ....... [[domain/repository-contract]]
  StoreProfileUseCase.kt .......... [[domain/usecase-contract]]
  StoreProfileUseCaseImpl.kt ...... [[domain/usecase-impl]]
  StoreProfileErrorIds.kt ......... [[domain/error-ids]]
data/
  StoreProfileRepositoryImpl.kt ... [[data/repository-impl]]
mapper/
  StoreProfileTransformer.kt ...... [[mapper/transformer]]
di/
  StoreProfileModule.kt ........... [[di/di-module]]
presentation/state/
  StoreProfileUiState.kt .......... [[presentation/state/state-uistate]]
  StoreProfileAction.kt ........... [[presentation/state/state-action]]
  StoreProfileEffect.kt ........... [[presentation/state/state-effect]]
  StoreProfileNavigation.kt ....... [[presentation/state/state-navigation]]
  StoreProfileInitialState.kt ..... [[presentation/state/state-initialstate]]
presentation/viewmodel/
  StoreProfileViewModel.kt ........ [[presentation/viewmodel/viewmodel]]
presentation/screen/
  StoreProfileRootScreen.kt ....... [[presentation/screen/screen-root]]
  StoreProfileScreen.kt ........... [[presentation/screen/screen-pure]]
presentation/navigation/
  StoreProfileRoutes.kt ........... [[presentation/navigation/nav-routes]]
  StoreProfileDestinations.kt ..... [[presentation/navigation/nav-destinations]]
  StoreProfileNavGraph.kt ......... [[presentation/navigation/nav-navgraph]]
build.gradle.kts .................. [[build-gradle]]
```

## What this feature deliberately leaves out (and why)

- **No `data/api`, `dto/`, `provider/`, `cache/`** — it reuses the already-cached
  `StoreSettings` from `:android-common`. When a real Store Profile endpoint arrives, that's
  where the network layer slots in (see [[data/repository-impl]]).
- **No `isLoading` flag** — view-only has no user-triggered actions, so only `InitialState`
  is needed (see [[presentation/state/state-initialstate]]).
- **No `entity/` / Room** — nothing is persisted by this feature.

## Suggested reading order (for a learner)

1. [[domain/model-store-profile]] → 2. [[domain/repository-contract]] →
3. [[domain/usecase-impl]] (see the `Result` + error conversion) →
4. [[data/repository-impl]] + [[mapper/transformer]] →
5. [[di/di-module]] (how it's all wired) →
6. [[presentation/state/state-initialstate]] →
7. [[presentation/viewmodel/viewmodel]] →
8. [[presentation/screen/screen-root]] + [[presentation/screen/screen-pure]] →
9. navigation files.
