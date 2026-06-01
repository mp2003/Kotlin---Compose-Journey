---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Repository impl wo file hai jo actually data laati hai aur use domain model me badal ke deti hai.

---

###### What
- File: `data/StoreProfileRepositoryImpl.kt`
- Interface implement karta hai
- Abhi data existing `SettingsRepository.getStoreSettings()` se aata hai (`:android-common`)
- `.map { it.toStoreProfile() }` -> domain model

---

###### Why
- Sirf data layer ko pata hota hai "data kahan se aaya"
- Aaj cache se, kal API se -> upar wali layers kuch nahi badlengi

---

###### How to write it (soch)
1. Source ko constructor me inject karo (`settingsRepository`, `transformer`)
2. Interface ka method override karo
3. Source call karo -> `Result` aaya
4. `.map { }` me source-type ko `StoreProfile` me convert karo (mapper se)

---

###### Syntax
- `: StoreProfileRepository` -> interface implement
- `with(transformer) { ... }` -> mapper ka extension scope me lao

```kotlin
class StoreProfileRepositoryImpl @Inject constructor(
    private val settingsRepository: SettingsRepository, // existing source
    private val transformer: StoreProfileTransformer,   // shape converter
) : StoreProfileRepository {

    override suspend fun getStoreProfile() =
        settingsRepository.getStoreSettings()           // Result<StoreSettings,_>
            .map { with(transformer) { it.toStoreProfile() } } // -> StoreProfile
}
```

> Note: `:android-common` apne aap mil jaata hai kyunki convention plugin use add karta hai.

---

###### Jab real API aayega (upgrade path)
- `data/api/` (Retrofit), `data/model/dto/`, `provider/` (Remote+Mock), `cache/` banao
- Yahan source `settingsRepository` se provider pe switch karo
- Real-vs-Mock ka faisla `di/` me hoga

---

###### Common Mistakes
- BAD: yahan mock-key check karna (di ka kaam)
- BAD: source type (`StoreSettings`) ko hi return karna (model me convert karo)

---

###### Memory Hook
- "Impl = source bula, mapper se model bana, de do. Bas."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `: Interface` | Class us interface ko implement karti hai |
| `@Inject constructor` | Hilt dependencies deta hai |
| `.map { }` | Result ka success value transform |
| `with(obj) { }` | obj ko receiver bana ke uske extensions use karo |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[02 - Phase 2 - Repository]]
- [[07 - Mapper Transformer]]
