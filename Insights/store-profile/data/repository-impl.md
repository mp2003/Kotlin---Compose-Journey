# StoreProfileRepositoryImpl.kt — repository implementation

**Code:** `.../data/StoreProfileRepositoryImpl.kt`

## Practice
- `@Inject constructor(settingsRepository, transformer)` implements the `domain/` interface.
- For this scaffold it sources data from the **existing** `SettingsRepository.getStoreSettings()`
  (in `:android-common`), then `.map { it.toStoreProfile() }` to the domain model.

## Why
- The data layer is the *only* layer that knows where data comes from. Today: a shared cache.
  Tomorrow: a Retrofit endpoint — and nothing above this file changes.
- `:android-common` is reachable because the `onePharma.retailer.impl` convention plugin adds it
  automatically (see `build-logic/.../AndroidImpl.kt`).

## Rule (ARCHITECTURE.md)
- `data/` holds `[Feature]RepositoryImpl` + (when needed) `provider/`, `cache/`, `api/`, models.
- The repository **must not know** whether it got a real or mock source — that's decided in `di/`.

## When the real API arrives (the upgrade path)
1. Add `data/api/StoreProfileApi.kt` (Retrofit `@GET`), `data/model/dto/StoreProfileDto.kt`,
   `data/model/response/…Response.kt`.
2. Add `provider/` (Remote + Mock) and optionally `cache/`.
3. Swap the source here from `settingsRepository` to the provider.
4. Select Remote-vs-Mock in `di/` via `KEY_STORE_PROFILE_MOCKED`.

## See also
- [[mapper/transformer]], [[domain/repository-contract]], [[di/di-module]].
