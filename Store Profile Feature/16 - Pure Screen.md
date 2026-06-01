---
up: "[[00 - Store Profile - Overview]]"
---

###### Elevator Pitch
- Pure Screen ek aisa composable hai jo sirf state leta hai aur callbacks deta hai — koi ViewModel nahi, isliye preview/test easy.

---

###### What
- File: `presentation/screen/StoreProfileScreen.kt`
- Signature: `StoreProfileScreen(uiState, modifier, onBack)`
- `@Preview` hai (kyunki pure hai -> fake state se chal jaata)

---

###### Why
- Pure composable = input ka function -> easy to preview/test
- Saari wiring upar RootScreen me -> ye file clean

---

###### How to write it (soch)
- Param: `uiState` (in) + callbacks like `onBack` (out) -> ye "state hoisting" hai
- `uiState` ko padho aur draw karo, kuch fetch/observe mat karo
- End me ek `@Preview` banao fake state ke saath

```kotlin
@Composable
fun StoreProfileScreen(
    uiState: StoreProfileUiState,      // state in
    modifier: Modifier = Modifier,
    onBack: () -> Unit = {},           // callback out
) {
    // sirf uiState.profile ko draw karo
}

@Preview
@Composable
private fun Preview() {
    StoreProfileScreen(uiState = StoreProfileUiState(profile = /* fake */))
}
```

> Design system: `design.andromedacompose.components.Text` + `AndromedaTheme.typography.*`. Valid styles: `titleHeroTextStyle`, `titleModerateDemiTextStyle`, `titleModerateBoldTextStyle`, `bodyModerateDefaultTypographyStyle`. `titleModerateTextStyle` EXIST NAHI karta (maine ye error khaaya tha).

---

###### Common Mistakes
- BAD: yahan `hiltViewModel()` ya flow collect karna (wo RootScreen me)
- BAD: galat typography name autocomplete bharose likhna

---

###### Memory Hook
- "Pure Screen = state do, draw lo. Kuch socho mat."

---

###### Keywords Used

| Keyword | Kya karta hai |
|---------|---------------|
| `@Composable` | Compose UI function |
| `@Preview` | IDE me bina app run kiye preview |
| `() -> Unit` | Callback lambda (no arg, no return) |
| state hoisting | State bahar se aata, event bahar jaata |

---

###### Related
- [[00 - Store Profile - Overview]]
- [[15 - RootScreen]]
- [[09 - UiState]]
