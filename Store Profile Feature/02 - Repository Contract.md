---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Repository contract ek interface hai jo bolta hai "data milega, par kaise milega ye baad ki baat."

---

###### What
- File: `domain/StoreProfileRepository.kt`
- Ek `interface` -> method `getStoreProfile(): Result<StoreProfile, DataError>`
- Ek mock-key constant bhi: `KEY_STORE_PROFILE_MOCKED`

---

###### Why
- Interface `domain/` me, impl `data/` me -> ye ==Dependency Inversion== hai
- UseCase interface pe depend karta hai, concrete class pe nahi -> source easily swap

---

###### How to write it (soch)
- Pucho: "is feature ko data ke liye kaun se kaam chahiye?" -> har kaam = ek method
- Return type hamesha `Result<Data, DataError>` -> success/fail dono explicit
- `suspend` lagao kyunki data laana async hai

---

###### Syntax
- `interface` -> sirf contract, body nahi
- `suspend` -> coroutine ke andar chalne wala function
- `Result<D, E>` -> Success(data) ya Error(error), throw nahi

```kotlin
interface StoreProfileRepository {
    // suspend = async; Result = explicit success/fail
    suspend fun getStoreProfile(): Result<StoreProfile, DataError>
}
```

> Yaad rakho: ye project ka `com.one.pharma.core.result.Result` hai, `kotlin.Result` NAHI.

---

###### Common Mistakes
- BAD: `kotlin.Result` use karna
- BAD: exception throw karke control flow chalana
- BAD: impl me mock-key check karna (wo di ka kaam hai)

---

###### Memory Hook
- "Contract bolta hai KYA, impl bolta hai KAISE."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `interface` | Sirf method signatures, koi body nahi |
| `suspend` | Async function, coroutine me chalta hai |
| `Result<D, E>` | Success ya Error wrapper (no throw) |
| `const val` | Compile-time constant (mock key) |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[02 - Dependency Inversion]]
- [[06 - Repository Impl]]
