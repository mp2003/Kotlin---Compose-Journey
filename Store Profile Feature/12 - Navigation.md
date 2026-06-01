---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Navigation ek alag sealed class hai jisme sirf navigate karne wale commands hote hain.

---

###### What
- File: `presentation/state/StoreProfileNavigation.kt`
- `sealed class` -> sirf `NavigateBack`
- RootScreen Effect ko isme convert karke upar bhejta hai

---

###### Why
- "navigate karna" (command) aur "NavController kaise karega" (Destination) alag rehte
- Screen NavController se decoupled -> preview/test easy

---

###### How to write it (soch)
- Effect me se sirf nav wale events nikaalo -> alag class
- Yahan toast/haptic NAHI aate, sirf navigate commands

```kotlin
sealed class StoreProfileNavigation {
    data object NavigateBack : StoreProfileNavigation()
}
```

---

###### Do sealed class kyun? (Effect + Navigation)
- `Effect.NavigateBack` = ViewModel bolta "nav event hua"
- `Navigation.NavigateBack` = RootScreen bolta "Destination, ab pop karo"
- NavController sirf Destination ke paas -> Screen/ViewModel clean

---

###### Common Mistakes
- BAD: Effect aur Navigation ko ek hi class bana dena
- BAD: NavController ko ViewModel/Screen me ghusana

---

###### Memory Hook
- "Effect = sab one-shot. Navigation = unme se sirf 'jao' wale."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `sealed class` | Fixed nav command types |
| `data object` | Bina data wala command |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[11 - Effect]]
- [[18 - Destinations]]
