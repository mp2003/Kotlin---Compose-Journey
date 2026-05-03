###### How to Use This Template

This file is both the template and the rulebook. When you paste raw study content into a Claude session, Claude must reshape it into this exact structure.

**Mandatory sections — every note must have these three:**
- Elevator Pitch
- Definition
- Memory Hook

**Optional sections — include only when they apply:**
- ASCII Flowchart → only if the concept has a flow or sequence
- Layered Diagram → only for architecture-level topics (MVI, Hilt layers, etc.)
- Weak Area Clarification → only when there is a confusing edge you keep getting wrong

**Hard rules (do not break):**
- No emojis anywhere. Use plain text markers like `[ x ]`, `->`, `OK`, `BAD` if needed.
- Real-world analogies are required — one dedicated "Real-World Analogy" block is mandatory, and you may also weave analogies inline inside What / Why / How it Works.
- Code examples must have **line-by-line comments in plain English**, no jargon. Write comments the way you would explain to a 12-year-old.
- Build code with **small blocks during the explanation**, then a single **Full Example** block at the end with all comments preserved.
- ASCII flowcharts go inside ```` ```text ```` blocks. Use boxes (`[ ]`), arrows (`->`, `|`, `v`), and simple labels.
- All section headings use `######` (h6). Separate every section with `---`.
- Link related notes with `[[Note Name]]` (Obsidian wiki-links).
- Plain language only. If a technical word appears, explain it in the same sentence.
- No filler sentences. Every line must teach something.

**Self-check before saving (read each line, mentally tick it off):**
- [ ] Elevator Pitch present and is one sentence
- [ ] Definition is formal and one line
- [ ] Real-World Analogy is present
- [ ] At least one Common Mistake listed
- [ ] At least one Common Follow-up Trap listed
- [ ] Memory Hook present and short
- [ ] Code (if any) has line-by-line plain-English comments
- [ ] Full Example block at the end if any code was built up
- [ ] No emojis anywhere
- [ ] Wiki-links to related notes in the Related section

---

###### Elevator Pitch
- <One sentence you would say in an interview if asked "what is this?". Plain language, no jargon.>

---

###### Definition
- <One formal one-line definition. Textbook style.>

---

###### Real-World Analogy
- <Compare the concept to something from daily life: a restaurant, a postman, a remote control, a recipe, a delivery service. Make it visual.>
- <One or two sentences max. The reader must "see" it.>

---

###### What
- <Plain-language bullet 1 — what this thing actually is>
- <Plain-language bullet 2>
- <Plain-language bullet 3>
- <Optional bullet 4>

---

###### Why
- <Benefit 1 — why anyone bothers using this>
- <Benefit 2>
- <Benefit 3>

---

###### Core Concepts
- <Concept 1> -> <one-line plain explanation>
- <Concept 2> -> <one-line plain explanation>
- <Concept 3> -> <one-line plain explanation>
- Link out: [[Related Note 1]], [[Related Note 2]]

---

###### How it Works
1. <Step 1 in plain language>
2. <Step 2>
3. <Step 3>
4. <Step 4>

---

###### ASCII Flowchart *(optional — include only when there is a flow)*

```text
[ User Action ]
       |
       v
[ ViewModel ]
       |
       v
[ Repository ]
       |
       v
[ Data Source ]
```

---

###### Layered Diagram *(optional — include only for architecture-level topics)*

```text
+-----------------------------+
|     Presentation Layer      |   <- UI, ViewModel
+-----------------------------+
|        Domain Layer         |   <- Business rules, UseCases
+-----------------------------+
|         Data Layer          |   <- Repository, ApiService
+-----------------------------+
```

---

###### Example (Step-by-step)

**Step 1 — <what this step accomplishes in one line>**

```kotlin
// Declare the data the screen will display.
// 'data class' = a class made just for holding values.
data class UiState(
    val items: List<String> = emptyList(), // the list shown to the user
    val isLoading: Boolean = false,        // true while we are fetching
    val error: String? = null              // holds an error message, or null
)
```

<Short prose explaining what just happened. One or two sentences. No filler.>

**Step 2 — <next step>**

```kotlin
// Update the state safely without changing the old one.
// '.copy()' creates a new object with the changed field.
_state.update { current ->
    current.copy(items = current.items + "New Item")
}
```

<Short prose linking step 2 back to step 1.>

---

###### Full Example

```kotlin
// =========================================================
// Full pattern: how the pieces fit together end-to-end.
// Read this top to bottom — every line is annotated.
// =========================================================

// 1. The shape of what the screen shows.
data class UiState(
    val items: List<String> = emptyList(), // visible list
    val isLoading: Boolean = false,        // spinner flag
    val error: String? = null              // error text or null
)

// 2. The thing that holds and updates the state.
class MyViewModel : ViewModel() {

    // _state is private so only this class can change it.
    private val _state = MutableStateFlow(UiState())

    // state is public read-only — UI observes this.
    val state: StateFlow<UiState> = _state.asStateFlow()

    // Called when the user does something.
    fun addItem(item: String) {
        _state.update { current ->
            // Make a NEW state with the extra item, do not mutate.
            current.copy(items = current.items + item)
        }
    }
}
```

---

###### Common Mistakes
- BAD: <mistake 1 — written so the reader sees the trap>
- BAD: <mistake 2>
- BAD: <mistake 3>

---

###### Weak Area Clarification *(optional — include only when there is a confusing edge)*
- <The exact place beginners (you) get tripped up>
- <Why the confusion happens>
- <The clearest one-line resolution>

---

###### Common Follow-up Traps
- Q: <follow-up question an interviewer asks after the basic one>
  A: <one-line answer>
- Q: <another follow-up>
  A: <one-line answer>
- Q: <a "why not the alternative?" question>
  A: <one-line answer>

---

###### Memory Hook
- <Short mnemonic, acronym, or vivid phrase. Examples: "State is the photo, Intent is the click, Effect is the sound." or "DI = Don't build it, demand it.">

---

###### Key Rule
- <One-line takeaway the reader should remember forever.>

---

###### Related
- [[Note 1]]
- [[Note 2]]
- [[Note 3]]
