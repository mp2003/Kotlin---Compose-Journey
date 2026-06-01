# StoreProfileModule.kt — Hilt DI module

**Code:** `.../di/StoreProfileModule.kt`

## Practice
- `@Module @InstallIn(ViewModelComponent::class) abstract class` with `@Binds`:
  - `StoreProfileRepositoryImpl → StoreProfileRepository`
  - `StoreProfileUseCaseImpl → StoreProfileUseCase`

## Why
- `@Binds` (not `@Provides`) because we're just telling Hilt "when someone asks for the
  interface, give this impl." `@Binds` is leaner — no method body, generated more efficiently.
- `ViewModelComponent` scope: these live as long as the ViewModel that needs them, not the whole app.

## Rule (ARCHITECTURE.md)
- `di/` wires Hilt. Source selection (mock vs real) is decided **here** via RemoteConfig.
- Prefer `@Provides`/`@Binds` over `@Named` where it works.

## `@Binds` vs `@Provides` (learner note)
- `@Binds`: interface → existing impl. Abstract fn, param = impl, return = interface.
- `@Provides`: you construct the object yourself (e.g. `retrofit.create(Api::class.java)`),
  needed for types you don't own. When the real API is added, the api/provider/cache modules
  will use `@Provides`.

## See also
- [[domain/repository-contract]], [[domain/usecase-contract]]. Real-API modules would add
  `StoreProfileApiModule` / `StoreProfileProviderModule` here (see [[data/repository-impl]]).
