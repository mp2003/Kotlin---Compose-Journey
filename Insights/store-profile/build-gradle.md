# build.gradle.kts — the module build file

**Code:** `features/android-retailer-store-profile-impl/build.gradle.kts`

## Practice
- `plugins { alias(libs.plugins.onePharma.retailer.impl); alias(libs.plugins.kotlin.serialization) }`
- An `android { … namespace = "io.one.pharma.retailer.store.profile.impl" }` block (build types,
  packaging excludes).
- `dependencies { }` — only module-specific extras (serialization-json, datetime).

## Why
- Almost all config (compileSdk 36, minSdk 26, Java 11, Compose, Hilt, the `network`/`andromeda`/
  `loggers` bundles, `:android-retailer-core`, `:android-retailer-resources`, `:android-common`)
  is injected by the **`onePharma.retailer.impl` convention plugin**. The module file stays tiny.
- This is why I could depend on `SettingsRepository` (`:android-common`) without adding a line —
  the plugin already wires it.

## Rule (ARCHITECTURE.md / practice #12)
- Don't duplicate `android {}` boilerplate or hardcode versions/SDKs per module. Change those in
  `build-logic/` and `gradle/libs.versions.toml`. A feature build file = `plugins {}` + extras.

## What I removed vs the stock-audit copy (and why)
- **Dropped `onePharma.room`** — view-only, nothing persisted.
- **Dropped** `:android-retailer-inventory-impl`, `:infra:android-mlkit-scanner`,
  `skydoves.flexible.bottomsheet` — those were stock-audit-specific (scanner/batch UI).
- **Changed** `namespace` to this module's package.

## Registration (separate file)
- The module must also be added to root `settings.gradle.kts`:
  `include(":features:android-retailer-store-profile-impl")` — else Gradle/IDE don't see it
  (that's why the folders showed grey/un-nested before sync).

## See also
- [[README]] (feature overview), [[di/di-module]] (what the plugin's Hilt setup enables).
