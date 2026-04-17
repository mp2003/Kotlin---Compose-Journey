- Represents user actions
- Sent from UI to ViewModel
- Contains no logic
- Describes what happened
- ViewModel decides behavior

###### What
- A representation of user interaction

###### Why
- Decouples UI from logic
- Centralizes actions
- Improves predictability

###### Core Concepts
- Defined as sealed class
- Each action = one intent
- Passed to ViewModel
- No business logic inside

###### How it Works
- User interacts
- UI sends intent
- ViewModel receives intent
- ViewModel processes it


###### Example (Kotlin)
```kotlin
sealed class UiIntent {
    object Load
    data class Add(val item: String)
    data class Delete(val item: String)
}
```
