---
up: "[[00 - Networking - Overview]]"
---

###### Elevator Pitch
- Phase 3 connects the repository to Compose using MVI so the screen reacts to state instead of doing work itself.

---

###### Definition
- MVI is a UI pattern where the screen sends intents, the ViewModel updates state, and the UI renders that state.

---

###### Real-World Analogy
- Intent -> a request from the user
- ViewModel -> the coordinator that decides what to do
- UiState -> the current status shown on the screen

---

###### What
- `PostIntent` -> describes actions like loading posts
- `PostUiState` -> holds Loading, Success, Error, and Idle
- `PostViewModel` -> receives intents and updates state
- `PostScreen` -> reads state and draws the UI

---

###### Why
- Keeps the screen predictable
- Moves network work out of the composable
- Makes state changes easy to follow

---

###### Core Concepts
- Intent -> an action the UI asks for
- StateFlow -> holds the latest UI state
- Unidirectional data flow -> intent in, state out, UI re-renders

---

###### How it Works
- The user taps the load button
- `PostScreen` sends `PostIntent.LoadPosts`
- `PostViewModel` sets `Loading`
- The repository returns data or error
- The ViewModel emits `Success` or `Error`
- The UI redraws from state

---

###### Syntax
- `sealed class PostIntent` -> defines the actions the UI can send
- `sealed class PostUiState` -> defines what the screen can show
- `MutableStateFlow<PostUiState>` -> holds mutable screen state
- `val uiState: StateFlow<PostUiState>` -> exposes read-only state
- `fun onIntent(intent: PostIntent)` -> receives UI actions

```kotlin
// ViewModel: receives intents and updates UI state
class PostViewModel(
    private val repository: PostRepository
) : ViewModel() {
    // Private mutable state owned by the ViewModel
    private val _uiState = MutableStateFlow<PostUiState>(PostUiState.Idle)

    // Public read-only state for the UI
    val uiState: StateFlow<PostUiState> = _uiState

    // Intent entry point from the UI
    fun onIntent(intent: PostIntent) {
        // Check which action the UI requested
        when (intent) {
            // Load posts action
            PostIntent.LoadPosts -> {
                // Show loading first
                _uiState.value = PostUiState.Loading
                // Run the network call off the main work path
                viewModelScope.launch {
                    // Ask the repository for data
                    val result = repository.getPosts()
                    // Map repository result to screen state
                    _uiState.value = when (result) {
                        is ApiResult.Success -> PostUiState.Success(result.data)
                        is ApiResult.Error -> PostUiState.Error(result.message)
                    }
                }
            }
        }
    }
}
```

---

###### Common Mistakes
- BAD: creating the ViewModel inside the composable
- BAD: calling the repository directly from the UI
- BAD: resetting success state immediately after setting it

---

###### Memory Hook
- Intent goes in, state comes out.

---

###### Key Rule
- Keep UI rendering separate from UI logic.

---

###### Related
- [[00 - Networking - Overview]]
- [[02 - Phase 2 - Repository]]
- [[04 - Phase 4 - Error Handling]]

