- Represents one-time UI actions
- Not part of persistent state
- Triggered by ViewModel
- UI reacts once
- Does not re-trigger on recomposition

##### What
- A one-time event for UI reaction

###### Why
- Separates transient actions from state
- Prevents repeated events
- Keeps state clean

###### Core Concepts
- Defined as sealed class
- Emitted via SharedFlow
- Collected in UI
- Not stored in state  

######  How it Works
- ViewModel emits effect
- UI collects effect
- UI performs action (toast/navigation)

###### Example (Kotlin)
```kotlin

sealed class UiEffect {

    data class ShowToast(val message: String)

    data class Navigate(val route: String)

}