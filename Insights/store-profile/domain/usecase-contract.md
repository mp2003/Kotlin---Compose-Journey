# StoreProfileUseCase.kt — use-case contract

**Code:** `.../domain/StoreProfileUseCase.kt`

## Practice
- An **interface** describing one piece of business behaviour: `getStoreProfile()`.
- Returns `Result<StoreProfile, UseCaseError>` — note the error type changed from `DataError`
  (repo) to `UseCaseError` (use case).

## Why
- The UseCase is the seam between data and presentation. The ViewModel talks to *this*, never
  to the repository directly.
- Splitting interface (`UseCase`) from impl (`UseCaseImpl`) lets Hilt bind a fake in tests.

## Rule (ARCHITECTURE.md)
- **Business logic goes in UseCases, not ViewModels.** Even when the logic is "just fetch and
  map errors," it belongs here so the ViewModel stays a thin state machine.

## Gotcha
- The error type difference is intentional: `DataError` is technical (network/storage/...),
  `UseCaseError` is user-facing (title + message + id). The conversion is the UseCase's whole job here.

## See also
- [[domain/usecase-impl]] (the conversion), [[domain/error-ids]], [[presentation/viewmodel/viewmodel]] (the caller).
