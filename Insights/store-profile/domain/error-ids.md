# StoreProfileErrorIds.kt — error identifiers

**Code:** `.../domain/StoreProfileErrorIds.kt`

## Practice
- An `object` of `const val` error ids, e.g. `PROFILE_FETCH_ERROR = "store_profile_fetch_error"`.

## Why
- A stable id per failure lets analytics, logging, and UI reference the *same* string without
  typos. Change the wording once, here.

## Rule (ARCHITECTURE.md)
- Error identifiers live in `[Feature]ErrorIds` constants — **never inline error strings** at
  call sites. (Same principle as RemoteConfig keys and string resources: no magic strings.)

## Gotcha
- This is the *id*, not the user-facing *message*. The message is built in the UseCase via
  `toUseCaseError(defaultMessage = …)`. Keep ids stable even if you reword messages.

## See also
- [[domain/usecase-impl]] (uses the id).
