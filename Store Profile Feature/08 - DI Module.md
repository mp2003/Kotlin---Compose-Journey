---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- DI module Hilt ko batata hai ki kaun sa impl class kis interface ke liye use karna hai.

---

###### What
- File: `di/StoreProfileModule.kt`
- `@Module @InstallIn(ViewModelComponent::class) abstract class`
- `@Binds` se: RepoImpl -> Repo, UseCaseImpl -> UseCase

---

###### Why
- `@Binds` chhota hai -> sirf "interface maango to ye impl do"
- `ViewModelComponent` scope -> jab tak ViewModel hai, tab tak ye dependencies

---

###### How to write it (soch)
1. `@Module` lagao -> Hilt ko batao "yahan bindings hain"
2. `@InstallIn(...)` -> kis scope me chahiye
3. Har interface ke liye ek `@Binds abstract fun(impl): Interface`

---

###### Syntax
- `@Module` -> bindings ka container
- `@InstallIn(X::class)` -> kis Hilt component me
- `@Binds` -> interface ko impl se jodo (abstract fun)

```kotlin
@Module
@InstallIn(ViewModelComponent::class)   // ViewModel jitni life
abstract class StoreProfileModule {

    @Binds  // jab Repo maango -> RepoImpl do
    abstract fun bindRepository(impl: StoreProfileRepositoryImpl): StoreProfileRepository

    @Binds  // jab UseCase maango -> UseCaseImpl do
    abstract fun bindUseCase(impl: StoreProfileUseCaseImpl): StoreProfileUseCase
}
```

---

###### @Binds vs @Provides (zaroori)
- `@Binds` -> interface ko apni impl se jodna (abstract, no body)
- `@Provides` -> jab object KHUD banana ho (jaise `retrofit.create(...)`)
- Real API aaya to api/provider module `@Provides` use karega

---

###### Common Mistakes
- BAD: `@Provides` use karna jahan `@Binds` kaafi tha
- BAD: galat scope (`SingletonComponent`) bina zaroorat

---

###### Memory Hook
- "Binds = jodo, Provides = banao."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `@Module` | Bindings ka container |
| `@InstallIn` | Kis Hilt scope me install ho |
| `@Binds` | Interface -> impl mapping (abstract) |
| `abstract class` | Body nahi, sirf declarations |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[06 - Hilt - Provides vs Binds]]
- [[10 - Hilt - Scopes]]
