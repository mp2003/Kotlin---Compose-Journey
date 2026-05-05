###### Elevator Pitch
- MVI is a pattern where user actions become Intents, Intents update a single State object, and the UI just renders whatever State says.

---

###### Definition
- A unidirectional UI architecture built on three pieces: State, Intent, and Effect

---

###### Real-World Analogy
- Like a vending machine
- You press a button (Intent)
- The machine updates its display (State)
- It plays a sound once (Effect)
- You only ever press buttons — you never reach inside

---

###### What
- A way to organize UI screens predictably
- One State object per screen
- Data flows in one direction only
- UI is "dumb" — just renders State

---

###### Why
- No scattered logic across UI files
- Easy to debug (one flow to follow)
- Easy to test (pure functions)
- Scales to large apps without chaos

---

###### Core Concepts
- [[01 - State - UI Data]] -> what the UI shows
- [[02 - Intent - User Actions]] -> what the user did
- [[05 - Effect - One-time Events]] -> one-time events (toast, navigation)
- Single Source of Truth -> one state object per screen
- Unidirectional Data Flow -> data moves one way only
- [[06 - ViewModel - The Brain]] -> the brain that ties it together

---

###### How it Works
- User taps something
- UI sends an [[02 - Intent - User Actions]]
- [[06 - ViewModel - The Brain]] receives it
- ViewModel updates [[01 - State - UI Data]]
- UI re-renders from new State
- ViewModel emits an [[05 - Effect - One-time Events]] for one-time things

---

###### ASCII Flowchart

```text
[ User ]
   |
   v  (Intent)
[ ViewModel ]
   |        \
   v         v  (Effect)
[ State ]  [ Toast / Navigation ]
   |
   v
[ UI re-renders ]
```

---

###### Common Mistakes
- BAD: UI calling ViewModel methods directly (bypasses Intent)
- BAD: Storing one-time events in State
- BAD: Multiple state objects per screen
- BAD: Mutating state instead of replacing it

---

###### Common Follow-up Traps
- Q: How is MVI different from MVVM?
  A: MVI has one State object and one Intent entry point; MVVM has multiple LiveData and direct method calls.
- Q: Why unidirectional?
  A: Easier to trace bugs — data only flows one way.
- Q: Is MVI overkill for small screens?
  A: Yes for trivial screens; great for screens with multiple states and actions.

---

###### Memory Hook
- "Intent in, State out, Effect once."

---

###### Key Rule
- UI never decides — it only renders State and sends Intents

---

###### Related
- [[01 - State - UI Data]]
- [[02 - Intent - User Actions]]
- [[05 - Effect - One-time Events]]
- [[06 - ViewModel - The Brain]]
- [[03 - onIntent - Entry Point]]
- [[04 - Reducer - Pure State Update]]
- [[07 - Task Manager Screen - Exercise]]
