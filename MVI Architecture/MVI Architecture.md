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
- [[State]] -> what the UI shows
- [[Intent]] -> what the user did
- [[Effect]] -> one-time events (toast, navigation)
- Single Source of Truth -> one state object per screen
- Unidirectional Data Flow -> data moves one way only
- [[ViewModel]] -> the brain that ties it together

---

###### How it Works
- User taps something
- UI sends an [[Intent]]
- [[ViewModel]] receives it
- ViewModel updates [[State]]
- UI re-renders from new State
- ViewModel emits an [[Effect]] for one-time things

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
- [[State]]
- [[Intent]]
- [[Effect]]
- [[ViewModel]]
- [[onIntent]]
- [[Reducer]]
