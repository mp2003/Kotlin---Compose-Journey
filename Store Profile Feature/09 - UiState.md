---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- UiState ek immutable object hai jo batata hai screen pe abhi kya-kya dikh raha hai.

---

###### What
- File: `presentation/state/StoreProfileUiState.kt`
- Ek `@Stable data class` -> yahan sirf `profile: StoreProfile?`

---

###### Why
- MVI ka "single source of truth" UI ke liye
- Immutable + `.copy()` -> Compose change pakad ke recompose karta hai

---

###### How to write it (soch)
- Pucho: "screen ko draw karne ke liye kya-kya chahiye?" -> wahi fields
- Default values do (`= null`, `= false`)
- `@Stable` lagao -> Compose ko hint, kam recompose

```kotlin
@Stable                       // Compose optimization hint
data class StoreProfileUiState(
    val profile: StoreProfile? = null,  // null = abhi load nahi hua
)
```

> View-only me sirf data. User-action wali screen me `isLoading`, `isRefreshing` jaise flags aate (par first-load ke liye nahi -> wo InitialState).

---

###### Common Mistakes
- BAD: toast/navigation State me daalna (wo Effect hai)
- BAD: list ko mutate karna (`.add`) -> `.copy()` use karo

---

###### Memory Hook
- "State = screen ki photo. Nayi photo, purani mat badlo."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `data class` | Immutable data holder |
| `@Stable` | Compose ko stable-equality hint -> fewer recomposes |
| `Type?` | Nullable -> value ya null |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[01 - State - UI Data]]
- [[13 - InitialState]]
