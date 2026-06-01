---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Mapper ek chhoti file hai jo ek data shape ko doosri shape me convert karti hai (source -> domain).

---

###### What
- File: `mapper/StoreProfileTransformer.kt`
- `@Inject` class with extension `StoreSettings.toStoreProfile(): StoreProfile`

---

###### Why
- Sirf mapper dono shapes ko jaanta hai
- Field add/remove hua -> sirf ek function badlega
- Injected class -> testable + swappable

---

###### How to write it (soch)
- Pucho: "source ka kaun sa field domain ke kaun se field me jaayega?"
- Ek extension function banao `Source.toDomain()`
- Har field map karo, return naya domain object

---

###### Syntax
- `Type.func()` -> extension function (Type pe naya method)
- `@Inject constructor()` -> Hilt banata hai

```kotlin
class StoreProfileTransformer @Inject constructor() {
    // extension: StoreSettings ke upar toStoreProfile()
    fun StoreSettings.toStoreProfile() = StoreProfile(
        storeName  = billStoreName, // source field -> domain field
        adminPhone = adminNo,
        themeColor = themeColor,
    )
}
```

---

###### Common Mistakes
- BAD: dto/entity ko hi domain model bana dena (mapper skip)
- BAD: mapping logic repo me bhar dena

---

###### Memory Hook
- "Mapper = translator. Source bole, domain sune."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| extension function | Kisi class pe bahar se naya method jodta hai |
| `@Inject constructor()` | Hilt is class ko bana sakta hai |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[01 - Domain Model]]
- [[06 - Repository Impl]]
