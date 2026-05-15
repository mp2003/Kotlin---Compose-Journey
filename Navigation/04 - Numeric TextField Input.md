---
up: "[[00 - Navigation - Overview]]"
---

###### Elevator Pitch
- To make a field accept only numbers you need two separate things: `KeyboardType.Number` to show the number keypad (a UX hint), and a digit-only filter in the ViewModel reducer that actually enforces it.

---

###### Definition
- Numeric input = a `TextField` with `keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)` plus validation in `onEvent` that rejects non-digits

---

###### Real-World Analogy
- `KeyboardType.Number` -> a sign saying "numbers only" on the door
- The ViewModel filter -> the bouncer who actually turns away anyone who isn't a number
- The sign alone does not stop anyone — the bouncer does

---

###### What
- `KeyboardOptions(keyboardType = KeyboardType.Number)` -> requests the numeric soft keyboard
- `event.value.all { it.isDigit() }` -> the real digit-only check
- Validation lives in the ViewModel `onEvent`, not in the UI `onValueChange`
- State only updates when input passes the filter

---

###### Why
- `KeyboardType.Number` is only a hint — paste or a hardware keyboard can still inject letters
- Real enforcement must be code that refuses bad input
- In MVI, all state changes go through the reducer, so validation belongs there
- Keeps the UI dumb and the rule in one testable place

---

###### Core Concepts
- `keyboardOptions` -> tells the system which keyboard layout to show
- `KeyboardType.Number` -> digits-only keypad (also `.Phone`, `.Decimal`, `.Email`)
- UX hint vs enforcement -> keyboard type suggests; the filter enforces
- Reducer validation -> `onEvent` decides whether/how state changes
- Link: [[00 - Navigation - Overview]]

---

###### How it Works
- User focuses the age field -> `KeyboardType.Number` shows the number pad
- User types/pastes -> `onValueChange` fires `MainUiEvent.AgeChanged(text)`
- ViewModel `onEvent` checks `text.all { it.isDigit() }`
- If all digits -> `_state.update { it.copy(age = text) }`
- If not -> state is left unchanged, the bad input is dropped

---

###### Syntax
- `keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)` -> numeric keypad
- `import androidx.compose.foundation.text.KeyboardOptions`
- `import androidx.compose.ui.text.input.KeyboardType`
- `value.all { it.isDigit() }` -> true only if every char is 0-9

```kotlin
// UI — only requests the keyboard, no validation here
TextField(
    value = state.age,
    label = { Text("age") },
    onValueChange = { viewModel.onEvent(MainUiEvent.AgeChanged(it)) },
    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
)

// ViewModel — the actual enforcement (MVI reducer)
is MainUiEvent.AgeChanged -> {
    if (event.value.all { char -> char.isDigit() }) {  // reject non-digits
        _state.update { it.copy(age = event.value) }   // only then update
    }
}
```

---

###### ASCII Flowchart

```text
[ User taps age field ]
        |
        v   KeyboardType.Number
[ Number keypad shown (UX hint only) ]
        |
        v   user types / pastes
[ onValueChange -> AgeChanged(text) ]
        |
        v
[ ViewModel: text.all { isDigit() } ? ]
   yes |          no |
       v             v
[ state.copy(age) ]  [ ignore, state unchanged ]
```

---

###### Common Mistakes
- BAD: relying on `KeyboardType.Number` alone -> letters still enter via paste / hardware keyboard
- BAD: putting the filter in the UI `onValueChange` in MVI -> validation belongs in the reducer
- BAD: testing on an emulator with hardware keyboard ON -> no soft keyboard appears at all
- BAD: using "Apply Changes" hot reload for `keyboardOptions` -> often not picked up; do a full Run
- BAD: parsing age with `toInt()` instead of `toIntOrNull() ?: 0` -> crash on empty string

---

###### Weak Area Clarification
- Confusion: "I set KeyboardType.Number but letters still get in / keypad not showing"
- Why: keyboard type is a hint, not validation; and emulator hardware-keyboard suppresses the soft keyboard
- Resolution: enforce with `isDigit()` in the ViewModel, and test on a real device or disable emulator hardware keyboard

---

###### Common Follow-up Traps
- Q: Does `KeyboardType.Number` prevent letters?
  A: No — it only changes the keyboard shown; validation must be in code
- Q: Where should the digit filter live in MVI?
  A: In the ViewModel `onEvent` reducer — all state transitions go through it
- Q: Number pad not showing on emulator, code looks right?
  A: Emulator is using the Mac hardware keyboard — disable it or test on a phone

---

###### Memory Hook
- Keyboard type is the sign, the reducer is the bouncer

---

###### Key Rule
- `KeyboardType.Number` shows the keypad; only the reducer's `isDigit()` filter actually enforces digits

---

###### Related
- [[00 - Navigation - Overview]]
- [[01 - NavHost and NavController]]
