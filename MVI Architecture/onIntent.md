
- Represents entry point for all user actions
- UI sends actions through this function
- ViewModel receives and processes actions
- Routes actions to correct logic
- Keeps flow centralized and predictable

###### What
- A function that handles all user Intents in [[ViewModel]]

###### Why
- Avoids multiple public functions in ViewModel
- Centralizes all user actions
- Makes debugging and tracking easier
- Keeps UI simple and logic controlled

###### Core Concepts
- Single entry point function
- Accepts UiIntent as parameter
- Uses when(intent) for branching
- Calls internal functions
- Updates State or emits Effect

###### How it Works
- UI sends Intent
- onIntent() receives it
- when(intent) decides action
- ViewModel processes logic
- State updated or Effect emitted
- UI reacts

###### Example (Kotlin)
```kotlin
fun onIntent(intent: UiIntent) {
    when (intent) {

        is UiIntent.Add -> {
            addItem(intent.item)
        }

        is UiIntent.Delete -> {
            deleteItem(intent.item)
        }
    }
}