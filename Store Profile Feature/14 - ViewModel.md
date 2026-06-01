---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- ViewModel ek patla state machine hai: Action lo -> UseCase call karo -> State copy karo / Effect emit karo.

---

###### What
- File: `presentation/viewmodel/StoreProfileViewModel.kt`
- `@HiltViewModel ... : BaseViewModel()`
- 3 flows: `initialState`, `uiState`, `effect`
- `init { fetch() }`, `onAction(action)`, `fetch()` with `.onSuccess/.onError`

---

###### Why
- ViewModel patla rehna chahiye -> logic UseCase me
- `BaseViewModel` (android-common) se ready helpers: `showError`, `processResultState`, etc.

---

###### How to write it (soch)
1. `@HiltViewModel` + `@Inject constructor(useCase)`
2. 3 private MutableFlow banao, public read-only expose karo
3. `init {}` me first fetch trigger karo
4. `onAction` me `when(action)` -> har case handle
5. `fetch()` -> `viewModelScope.launch(IO)` -> usecase -> `.onSuccess/.onError`

```kotlin
@HiltViewModel
class StoreProfileViewModel @Inject constructor(
    private val useCase: StoreProfileUseCase,
) : BaseViewModel() {

    private val _initialState = MutableStateFlow<StoreProfileInitialState>(Loading)
    val initialState = _initialState.asStateFlow()          // public read-only

    private val _uiState = MutableStateFlow(StoreProfileUiState())
    val uiState = _uiState.asStateFlow()

    private val _effect = MutableSharedFlow<StoreProfileEffect>()
    val effect = _effect.asSharedFlow()

    init { fetch() }                                        // first load

    fun onAction(action: StoreProfileAction) = when (action) {
        OnBack -> triggerEffect(NavigateBack)
        OnScreenResumed -> fetch()
    }

    private fun fetch() = viewModelScope.launch(Dispatchers.IO) {
        useCase.getStoreProfile()
            .onSuccess { _uiState.update { s -> s.copy(profile = it) }
                         _initialState.value = Success }
            .onError { _initialState.value = Error(it.title ?: "Store Profile", it.message) }
    }
}
```

> Note: CLAUDE.md bolta "no BaseViewModel" — ye GALAT hai. BaseViewModel exist karta hai, features use karte hain. Code pe bharosa karo.

---

###### Common Mistakes
- BAD: business logic yahan likhna (UseCase me daalo)
- BAD: `_uiState` ko public expose karna (read-only `.asStateFlow()` do)
- BAD: SharedFlow ko composable body me collect (LaunchedEffect me karo)

---

###### Memory Hook
- "ViewModel = Action in, State/Effect out. Logic nahi, bas plumbing."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `@HiltViewModel` | Hilt is ViewModel ko bana sakta hai |
| `MutableStateFlow` | Badalne wala state holder (sticky) |
| `MutableSharedFlow` | One-shot events stream |
| `.asStateFlow()` | Read-only public version |
| `.update { copy() }` | Thread-safe immutable update |
| `viewModelScope.launch` | ViewModel ke coroutine me chalao |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[06 - ViewModel - The Brain]]
- [[03 - UseCase Contract]]
