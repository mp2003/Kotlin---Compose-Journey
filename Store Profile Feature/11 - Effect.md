---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Effect ek sealed class hai jo one-time events bhejti hai (toast, navigate) jo sirf ek baar hone chahiye.

---

###### What
- File: `presentation/state/StoreProfileEffect.kt`
- `sealed class` -> `ShowToast(message)`, `NavigateBack`
- `SharedFlow` se emit, RootScreen ek baar collect

---

###### Why
- State sticky hota (har recompose pe dikhe) -> toast har baar nahi chahiye
- Effect ek baar consume hota -> isliye toast/navigate Effect hain

---

###### How to write it (soch)
- Pucho: "kya ye ek baar hone wali cheez hai (toast/nav)?" -> Effect
- "kya ye screen pe rehne wali cheez hai?" -> State, Effect nahi
- Saare one-shot events (nav bhi) Effect me

```kotlin
sealed class StoreProfileEffect {
    data class ShowToast(val message: String) : StoreProfileEffect()
    data object NavigateBack : StoreProfileEffect()
}
```

---

###### Weak Area Clarification
- State vs Effect confuse?
- Rotation ke baad dobara dikhe? -> State
- Sirf ek baar fire ho (toast/haptic/nav)? -> Effect

---

###### Common Mistakes
- BAD: toast ko State me boolean banake rakhna
- BAD: Effect ko StateFlow se bhejna (SharedFlow use karo)

---

###### Memory Hook
- "State = rehne wali cheez, Effect = ek baar wali cheez."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `sealed class` | Fixed event types |
| `SharedFlow` | One-shot events stream (sticky nahi) |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[05 - Effect - One-time Events]]
- [[12 - Navigation]]
