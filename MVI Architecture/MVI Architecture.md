###### **What**

- A UI architecture where **user actions (Intent)** drive **state updates**, and UI renders from **State**

---
###### **Why**

- Avoids scattered logic across UI
- Prevents unpredictable UI behaviour
- Makes debugging easier (single flow)
- Helps scale large applications cleanly

---
###### **Core Concepts**

- [[State]] → represents UI data
- [[Intent]] → represents user actions
- [[Effect]] → represents one-time events
- **Single Source of Truth** → one state object per screen
- **Unidirectional Data Flow** → data flows in one direction

---
######  **How it Works**

- User interacts with UI
- UI sends [[Intent]]
- [[ViewModel]] receives Intent
- ViewModel processes logic
- ViewModel updates [[State]]
- UI observes State and re-renders
- ViewModel emits **Effect** for one-time actions

