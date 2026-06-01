---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Module ka build.gradle.kts bahut chhota hota hai — bas plugins + thodi extra deps, baaki sab convention plugin sambhalta hai.

---

###### What
- File: `features/android-retailer-store-profile-impl/build.gradle.kts`
- `plugins { onePharma.retailer.impl; kotlin.serialization }`
- `android { namespace = "...store.profile.impl" }`
- `dependencies { }` -> sirf module-specific extras

---

###### Why
- compileSdk, minSdk, Java, Compose, Hilt, android-common/core/resources -> sab ==convention plugin== se aata hai
- Isliye SettingsRepository bina extra line ke mil gaya

---

###### How to write it (soch)
- Reference module copy karo (stock-audit-impl)
- Sirf `namespace` apne package pe set karo
- Jo plugins/deps is feature ko nahi chahiye, hatao

```kotlin
plugins {
    alias(libs.plugins.onePharma.retailer.impl)   // saara android+hilt+compose setup
    alias(libs.plugins.kotlin.serialization)
}
android { namespace = "io.one.pharma.retailer.store.profile.impl" }
dependencies {
    implementation(libs.kotlinx.serialization.json)
    implementation(libs.kotlinx.datetime)
}
```

---

###### Maine stock-audit se kya hataya (aur kyun)
- `onePharma.room` hataya -> view-only, kuch persist nahi
- inventory-impl, mlkit-scanner, flexible-bottomsheet hataye -> wo stock-audit ke the
- `namespace` apne package pe set kiya

---

###### Registration (alag file)
- Root `settings.gradle.kts` me: `include(":features:android-retailer-store-profile-impl")`
- Warna Gradle/IDE module ko dekhega hi nahi (folders grey dikhte the)

---

###### Common Mistakes
- BAD: yahan `android {}` boilerplate duplicate karna
- BAD: versions hardcode karna (libs.versions.toml use karo)
- BAD: settings.gradle.kts me include bhoolna

---

###### Memory Hook
- "Feature gradle = plugin + naam + extras. Bas itna."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `plugins { alias(...) }` | Convention plugin lagao |
| `namespace` | Module ka package/R-class base |
| `implementation(project(...))` | Doosre module pe depend |
| convention plugin | Saara common config ek jagah |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[00 - DI - Overview]]
