# StoreProfileTransformer.kt — the mapper

**Code:** `.../mapper/StoreProfileTransformer.kt`

## Practice
- `@Inject` class with an extension `StoreSettings.toStoreProfile(): StoreProfile`.
- Lives in `mapper/`, separate from both layers it connects.

## Why
- The mapper is the **only** place that knows both shapes. It absorbs change: when the source
  type or the domain model gains/loses a field, you edit one function.
- Making it an injected class (not a top-level fn) keeps it testable and swappable via Hilt.

## Rule (ARCHITECTURE.md)
- `mapper/` transforms between layers. `dto → domain`, `entity → domain`, etc. Never reuse a
  `dto`/`entity` as a domain model to skip the mapper.

## Gotcha
- Used at the call site as `with(transformer) { storeSettings.toStoreProfile() }` because it's an
  extension defined *inside* a class — you bring the receiver into scope with `with`.

## See also
- [[data/repository-impl]] (the caller), [[domain/model-store-profile]] (the output).
