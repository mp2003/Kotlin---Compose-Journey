# StoreProfileRepository.kt — repository contract

**Code:** `.../domain/StoreProfileRepository.kt`

## Practice
- An **interface** in `domain/`, returning `Result<StoreProfile, DataError>`.
- Also holds the RemoteConfig mock-key constant: `KEY_STORE_PROFILE_MOCKED`.

## Why
- Interface in `domain/`, implementation in `data/` = **Dependency Inversion**. The UseCase
  depends on the abstraction, not the concrete class — so the data source can be swapped freely.
- Returning `Result` (not throwing) makes success/failure explicit and forces the caller to handle it.

## Rule (ARCHITECTURE.md)
- Repository methods **must** return `com.one.pharma.core.result.Result<T, E>` — never
  `kotlin.Result`, no exceptions for control flow.
- The repo returns **`DataError`**. Converting to a user-facing error is the UseCase's job.

## Gotcha
- The mock-vs-real choice is decided in `di/` using `KEY_STORE_PROFILE_MOCKED`. The interface
  exposes the key, but the **implementation must not check it** — it just uses whatever DI gave it.

## See also
- [[data/repository-impl]], [[domain/usecase-impl]] (the caller), [[di/di-module]] (where it's bound).
