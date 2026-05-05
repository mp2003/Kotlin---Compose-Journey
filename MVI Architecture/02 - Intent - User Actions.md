---
up: "[[00 - MVI - Overview]]"
---

###### Elevator Pitch
- Intent is a small data object the UI sends to the ViewModel to say "the user just did this thing."

---

###### Definition
- A sealed class where each subclass represents one possible user action

---

###### Real-World Analogy
- Like ringing a doorbell
- The bell does not decide who answers
- It just announces "someone is at the door"
- The person inside (ViewModel) decides what to do

---

###### What
- Represents one user action
- Sent from UI to ViewModel
- Carries no logic
- Just describes what happened

---

###### Why
- Decouples UI from business logic
- Centralizes all user actions in one type
- Makes the UI readable — every tap maps to one Intent
- Easy to log and replay

---

###### Core Concepts
- Defined as a sealed class (or sealed interface)
- One subclass = one action
- Data classes carry parameters, objects do not
- Always passed into `onIntent()`

---

###### How it Works
- User taps a button
- UI builds an Intent
- UI calls `viewModel.onIntent(intent)`
- ViewModel decides what to do

---

###### Example

```kotlin
// Sealed class = closed set of possible actions
sealed class UiIntent {
    object Load : UiIntent()                          // no parameters
    data class Add(val item: String) : UiIntent()     // carries the new item
    data class Delete(val item: String) : UiIntent()  // carries the item to remove
}
```

---

###### Common Mistakes
- BAD: Putting business logic inside Intent classes
- BAD: Calling ViewModel methods directly instead of sending an Intent
- BAD: Making Intent mutable

---

###### Common Follow-up Traps
- Q: Why sealed class instead of enum?
  A: Enums cannot carry per-action data; sealed classes can.
- Q: Where does the Intent get processed?
  A: In `onIntent()` inside the ViewModel.
- Q: Object vs data class for an Intent?
  A: Object for actions with no parameters, data class when you need to carry data.

---

###### Memory Hook
- "Intent = doorbell. Press it, don't open the door."

---

###### Key Rule
- Intent describes, never decides

---

###### Keywords Used

| Keyword | What it does |
|---------|--------------|
| `sealed class` | A closed class hierarchy — all subclasses must be declared in the same file |
| `object` | A singleton subclass of a sealed class — used for actions with no parameters |
| `data class` | A sealed subclass that carries data fields as constructor parameters |
| `: UiIntent()` | Inherits from the sealed class, making this subclass part of the sealed hierarchy |

---

###### Related
- [[00 - MVI - Overview]]
- [[03 - onIntent - Entry Point]]
