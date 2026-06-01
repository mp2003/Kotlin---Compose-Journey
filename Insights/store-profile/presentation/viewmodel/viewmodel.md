# StoreProfileViewModel.kt — the state machine

**Code:** `.../presentation/viewmodel/StoreProfileViewModel.kt`

## Practice
- `@HiltViewModel class … @Inject constructor(useCase) : BaseViewModel()`.
- Exposes three flows: `initialState` (StateFlow), `uiState` (StateFlow), `effect` (SharedFlow).
- `init { fetchStoreProfile() }` kicks off the one-time load.
- `onAction(action)` — single entry point; `when` over the sealed Action.
- `fetchStoreProfile()` calls the UseCase and branches with `.onSuccess { … } .onError { … }`,
  setting `_uiState.update { it.copy(...) }` and `_initialState.value = …`.

## Why
- The ViewModel is a **thin** state machine: receive Action → call UseCase → copy State / emit
  Effect. No business logic, no networking details.
- `BaseViewModel` (in `:android-common`) provides shared helpers: `showError/showLoading/
  showSuccess`, `processResultState`, `atomicLaunch`. Extending it keeps features consistent.

## Rule (ARCHITECTURE.md / practice #3, #6)
- Business logic in UseCases, not here. ViewModel only validates, copies state, triggers Effects.
- `initialState` vs `uiState` are separate flows for separate concerns (see [[presentation/state/state-initialstate]]).

## ⚠️ Note vs your earlier notes
- The repo's `CLAUDE.md` *claims* there's no `BaseViewModel` — that's outdated. It **does**
  exist (`com.one.pharma.common.base.BaseViewModel`) and features extend it. Trust the code.

## Flow helpers (learner note)
- `MutableStateFlow` (state, sticky) + `asStateFlow()` to expose read-only.
- `MutableSharedFlow` (effects, one-shot) + `asSharedFlow()`.
- `.update { it.copy(...) }` = thread-safe immutable update.
- `viewModelScope.launch(Dispatchers.IO)` for the suspend UseCase call.

## See also
- [[domain/usecase-impl]], [[presentation/screen/screen-root]] (collects these flows).
