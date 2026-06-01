# StoreProfileRootScreen.kt — the stateful host

**Code:** `.../presentation/screen/StoreProfileRootScreen.kt`

## Practice
- Takes `viewModel: StoreProfileViewModel = hiltViewModel()` and `onNavigationChange`.
- Collects flows with `collectAsStateWithLifecycle()`.
- `LaunchedEffect(Unit) { viewModel.effect.collectLatest { … } }` — handles toasts locally,
  forwards nav effects up via `onNavigationChange`.
- `Crossfade(initialState) { when(it) { Loading / Error → ErrorScreen / Success → StoreProfileScreen } }`.
- `BackHandler { onAction(OnBack) }`.

## Why
- This is the "impure" half of the screen — it touches the ViewModel, lifecycle, Effects,
  navigation. Isolating it here keeps the actual UI ([[presentation/screen/screen-pure]]) pure and testable.
- `collectAsStateWithLifecycle` stops collecting when the screen isn't visible (battery/correctness).

## Rule (ARCHITECTURE.md / practice #7, #8)
- RootScreen = stateful: collects Effect, sets up InitialState, handles navigation.
- It **forwards nav effects** but **handles non-nav effects** (toast) itself — the two-hierarchy split.

## Gotcha
- Effects use `collectLatest` inside `LaunchedEffect(Unit)` so collection survives recomposition
  but restarts only if the key changes. Don't collect a SharedFlow directly in composable body.

## See also
- [[presentation/screen/screen-pure]], [[presentation/state/state-effect]], [[presentation/state/state-navigation]], [[presentation/state/state-initialstate]].
