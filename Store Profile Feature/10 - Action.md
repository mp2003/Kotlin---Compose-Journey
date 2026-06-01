---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Action ek sealed class hai jo batati hai user/UI kya kar sakta hai.

---

###### What
- File: `presentation/state/StoreProfileAction.kt`
- `sealed class` -> `OnBack`, `OnScreenResumed`
- Screen ye actions `onAction(action)` ko bhejti hai

---

###### Why
- Saare input ek hi jagah se aate hain -> reason karna easy
- Sealed -> ViewModel ka `when` exhaustive (case bhoole to compile fail)

---

###### How to write it (soch)
- Pucho: "user is screen pe kya-kya kar sakta hai?" -> har cheez ek action
- Naam me batao KYA HUA (`OnBack`), KYA KARNA hai nahi (`popBackStack` nahi)
- Data chahiye to `data class`, warna `data object`

```kotlin
sealed class StoreProfileAction {
    data object OnBack : StoreProfileAction()          // back dabaya
    data object OnScreenResumed : StoreProfileAction() // screen wapas aayi
}
```

---

###### Common Mistakes
- BAD: action naam me logic ("popBackStack") -> wo ViewModel decide karega
- BAD: normal class use karna sealed ke jagah (exhaustive `when` toot jaata)

---

###### Memory Hook
- "Action = 'kya hua' ki khabar, 'kya karo' ka order nahi."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `sealed class` | Fixed set of subtypes -> exhaustive `when` |
| `data object` | Bina data wala single instance subtype |
| `data class` | Data wala subtype (jab argument chahiye) |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[02 - Intent - User Actions]]
- [[14 - ViewModel]]
