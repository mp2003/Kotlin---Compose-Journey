---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Domain model ek plain data class hai jo poore app me "store profile ka matlab kya hai" define karta hai.

---

###### What
- File: `domain/model/StoreProfile.kt`
- Ek simple `data class StoreProfile(...)`
- ==No framework imports== -> koi Android, Retrofit, Room nahi

---

###### Why
- Ye app ka stable core hai
- JSON ya DB badle -> sirf dto/entity + mapper badlega, ye file safe
- Framework-free -> bina Android ke test ho jata hai

---

###### How to write it (soch)
- Pucho: "screen ko dikhane ke liye kya-kya chahiye?" -> wahi fields
- Sirf `val` rakho (read-only)
- Koi annotation mat lagao (`@SerializedName`/`@Entity` yahan BAD)

---

###### Syntax
- `data class` -> immutable data holder, auto `copy()`/`equals()`
- `val` -> read-only property

```kotlin
// domain model = pure kotlin, no annotations
data class StoreProfile(
    val storeName: String,   // dukaan ka naam
    val adminPhone: String,  // admin number
    val themeColor: String,  // theme color hex
)
```

---

###### Common Mistakes
- BAD: dto ko hi UI me daal dena (model skip karna)
- BAD: yahan `@SerializedName` lagana (wo dto ka kaam hai)

---

###### Memory Hook
- "Model = app ki apni bhasha, network/DB ki nahi."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `data class` | Immutable class banata hai, `copy()`/`equals()` auto |
| `val` | Read-only property |
| `String` | Text value |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[07 - Mapper Transformer]]
- [[09 - UiState]]
