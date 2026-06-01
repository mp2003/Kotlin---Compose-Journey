---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- UseCase contract ek interface hai jo ek business kaam describe karta hai, aur error ko user-friendly bana ke deta hai.

---

###### What
- File: `domain/StoreProfileUseCase.kt`
- `interface` -> `getStoreProfile(): Result<StoreProfile, UseCaseError>`
- Dhyan do: error type ab `DataError` se `UseCaseError` ho gaya

---

###### Why
- ViewModel sirf UseCase se baat karta hai, repo se direct nahi
- Business logic yahan rehti hai -> ViewModel patla rehta hai

---

###### How to write it (soch)
- Pucho: "screen ko kaun sa ek kaam chahiye?" -> wahi method
- Repo ka error (`DataError`) technical hota hai -> yahan `UseCaseError` (user-facing) banega
- Interface + Impl alag -> test me fake daal sakte ho

---

###### Syntax
- `UseCaseError` -> id + title + message wala user-facing error

```kotlin
interface StoreProfileUseCase {
    // note: UseCaseError, not DataError
    suspend fun getStoreProfile(): Result<StoreProfile, UseCaseError>
}
```

---

###### Weak Area Clarification
- DataError vs UseCaseError me confuse hote ho?
- DataError = technical (network/storage) -> repo deta hai
- UseCaseError = user ko dikhane layak (title+message) -> usecase banata hai

---

###### Common Mistakes
- BAD: business logic ViewModel me likhna
- BAD: repo ka `DataError` seedha UI tak le jaana

---

###### Memory Hook
- "UseCase = data aur screen ke beech ka pul + error translator."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `interface` | Contract only |
| `UseCaseError` | User-facing error (id, title, message) |
| `DataError` | Technical error from data layer |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[04 - UseCase Impl]]
- [[06 - ViewModel - The Brain]]
