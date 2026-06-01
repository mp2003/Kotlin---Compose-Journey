---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- InitialState ek sealed class hai jo first screen-load ka status batati hai: Loading, Success, ya Error.

---

###### What
- File: `presentation/state/StoreProfileInitialState.kt`
- `sealed class` -> `Loading`, `Success`, `Error(title, message)`
- Pehli baar screen kholne pe: spinner -> content / error

---

###### Why
- First load ke 3 outcomes ke 3 alag full-screen UI chahiye
- Sealed -> RootScreen ka `when` clean + exhaustive

---

###### How to write it (soch)
- 3 states socho: load ho raha / mil gaya / fail
- Error me title+message rakho taaki error screen bana sako

```kotlin
sealed class StoreProfileInitialState {
    data object Loading : StoreProfileInitialState()
    data object Success : StoreProfileInitialState()
    data class Error(val title: String, val message: String) : StoreProfileInitialState()
}
```

---

###### Weak Area Clarification (sabse confusing rule)
- InitialState != isLoading
- InitialState = pehla screen-load (ye file)
- isLoading = user ne kuch dabaya (refresh/save) tab ka flag, UiState me
- View-only me sirf InitialState chahiye, isLoading nahi

---

###### Common Mistakes
- BAD: first load ko `uiState.isLoading = true` se dikhana
- BAD: Error me sirf message, title bhula dena

---

###### Memory Hook
- "InitialState = screen boot. isLoading = button busy."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `sealed class` | Loading/Success/Error fixed set |
| `data object` | Bina data wala state |
| `data class` | Data wala state (Error me title/message) |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[09 - UiState]]
- [[15 - RootScreen]]
