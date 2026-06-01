# StoreProfileUseCaseImpl.kt — use-case implementation

**Code:** `.../domain/StoreProfileUseCaseImpl.kt`

## Practice
- `@Inject constructor(repository)` — Hilt builds it.
- Calls the repo, then `.map(onError = …, onSuccess = …)` to convert `DataError → UseCaseError`
  via `dataError.toUseCaseError(errorId = …, title = …, defaultMessage = …)`.

## Why
- One place owns "what the user sees when this fails." The id comes from `StoreProfileErrorIds`
  (stable, machine-readable); the message is human-friendly.
- `.map` keeps it a pure transformation on the `Result` — no `if/else`, no throwing.

## Rule (ARCHITECTURE.md)
- UseCases convert `DataError → UseCaseError` with user-facing messages; **error identifiers go
  in `[Feature]ErrorIds` constants, never inline strings.**

## Gotcha
- `Result.map` here takes BOTH `onError` and `onSuccess`. Forgetting `onError` would leave the
  error type as `DataError` and the call wouldn't compile against the `UseCaseError` contract.

## See also
- [[domain/error-ids]], [[domain/usecase-contract]], `toUseCaseError` lives in `:android-core` `result/error/ErrorUtils.kt`.
