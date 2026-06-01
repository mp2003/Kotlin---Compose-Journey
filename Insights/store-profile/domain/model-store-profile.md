# StoreProfile.kt — domain model

**Code:** `features/android-retailer-store-profile-impl/.../domain/model/StoreProfile.kt`

## Practice
- The **domain model** — the shape the rest of the app (UseCase, ViewModel, UI) speaks in.
- A plain `data class`. **No framework imports** (no Android, Retrofit, `@SerializedName`, `@Entity`).

## Why
- Domain is the stable core. If network JSON or DB schema changes, only the `dto`/`entity`
  + mapper change — the domain model and everything above it stay put.
- Framework-free ⇒ unit-testable without Android.

## Rule (ARCHITECTURE.md)
- `domain/model/` holds framework-free models. `dto` ≠ `entity` ≠ domain model; the mapper bridges.

## Gotcha
- Don't render a `dto` directly in the UI to "save a class." That couples your screen to the
  network shape — the exact thing this split prevents.

## See also
- [[mapper/transformer]] (how a source type becomes this), [[presentation/state/state-uistate]] (where the UI holds it).
