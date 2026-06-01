---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Error Ids ek object hai jisme har error ka ek fixed naam (id) hota hai, taaki kahin bhi same id use ho.

---

###### What
- File: `domain/StoreProfileErrorIds.kt`
- Ek `object` with `const val` ids
- Example: `PROFILE_FETCH_ERROR = "store_profile_fetch_error"`

---

###### Why
- Ek jagah saare error ids -> typo nahi hoti
- Analytics/logging/UI sab same id reference karte hain

---

###### How to write it (soch)
- Har fail case ke liye ek id banao
- Naam clear rakho -> `FEATURE_ACTION_ERROR`
- `object` use karo (single instance, no constructor)

---

###### Syntax
- `object` -> ek hi instance, sab static jaisa
- `const val` -> compile-time fixed string

```kotlin
object StoreProfileErrorIds {
    const val PROFILE_FETCH_ERROR = "store_profile_fetch_error"
}
```

> Yaad rakho: ye sirf ID hai, user ko dikhne wala message NAHI. Message UseCase me banta hai.

---

###### Common Mistakes
- BAD: id string ko har jagah inline likhna (magic string)
- BAD: id aur message ko mix karna

---

###### Memory Hook
- "Id = error ka aadhaar number — fixed aur ek hi jagah."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `object` | Single instance class (no `new`) |
| `const val` | Compile-time constant |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[04 - UseCase Impl]]
