---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- UseCase impl repo ko call karta hai aur uske technical error ko user-friendly message me badal deta hai.

---

###### What
- File: `domain/StoreProfileUseCaseImpl.kt`
- `@Inject constructor(repository)` -> Hilt banata hai
- Repo call karke `.map(onError, onSuccess)` se `DataError -> UseCaseError`

---

###### Why
- Ek hi jagah decide hota hai "fail hua to user ko kya dikhe"
- `.map` -> pure transformation, koi if/else ya throw nahi

---

###### How to write it (soch)
1. Repo ko constructor me inject karo
2. Method ke andar `repository.getStoreProfile()` call karo
3. `.map { ... }` lagao -> success aage bhejo, error convert karo
4. Error id `StoreProfileErrorIds` se lo (string inline mat likho)

---

###### Syntax
- `@Inject constructor(...)` -> Hilt is class ko bana sakta hai
- `.map(onError = {}, onSuccess = {})` -> Result transform
- `.toUseCaseError(...)` -> DataError ko UseCaseError banata hai

```kotlin
class StoreProfileUseCaseImpl @Inject constructor(
    private val repository: StoreProfileRepository, // Hilt injects
) : StoreProfileUseCase {

    override suspend fun getStoreProfile() =
        repository.getStoreProfile().map(
            onError = { it.toUseCaseError(            // tech -> user error
                errorId = StoreProfileErrorIds.PROFILE_FETCH_ERROR,
                title = "Store Profile",
                defaultMessage = "Could not load. Try again.",
            ) },
            onSuccess = { it },                       // data as-is
        )
}
```

---

###### Common Mistakes
- BAD: `onError` bhoolna -> error type convert nahi hoga -> compile fail
- BAD: error message string seedha yahan likhna (id constant use karo)

---

###### Memory Hook
- "Impl = repo bula, error ko Hindi me samjha (user-friendly), aage bhej."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `@Inject constructor` | Hilt ko batata hai class kaise banani hai |
| `override` | Interface ka method implement |
| `.map` | Result ke success/error dono transform |
| `.toUseCaseError()` | DataError -> UseCaseError converter |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[01 - Injection - Constructor Injection]]
- [[05 - Error Ids]]
