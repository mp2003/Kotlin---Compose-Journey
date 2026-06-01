---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Routes file me feature ke saare navigation route strings ek typed jagah par hote hain.

---

###### What
- File: `presentation/navigation/StoreProfileRoutes.kt`
- `object` with `Graph` (feature entry) aur `StoreProfile` (screen)
- Dono `SimpleRoute("...")` extend karte

---

###### Why
- Route strings ek jagah -> magic strings nahi
- `Graph` = bahar se entry, `StoreProfile` = andar ki screen

---

###### How to write it (soch)
- Feature ka ek graph route socho + har screen ka ek route
- Bina argument wala route -> `SimpleRoute`
- Argument wala hota -> `ArgumentRoute` (yahan nahi chahiye)

```kotlin
object StoreProfileRoutes {
    data object Graph : SimpleRoute("store_profile_graph")  // feature entry
    data object StoreProfile : SimpleRoute("store_profile") // screen
}
```

---

###### Graph route vs screen route
- Nested graph ka apna route hota hai, screens se alag
- Bahar wale `Graph` pe jaate -> graph andar `StoreProfile` dikhata
- Isliye DO routes

---

###### Common Mistakes
- BAD: route string ko jagah-jagah hardcode karna
- BAD: graph aur screen ka same route rakhna

---

###### Memory Hook
- "Graph = darwaza, Screen = kamra."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `object` | Single instance container |
| `data object` | Bina data wala route |
| `SimpleRoute` | Bina-argument route ka base class |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[05 - Nested Graphs]]
- [[19 - NavGraph]]
