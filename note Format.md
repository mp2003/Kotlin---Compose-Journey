###### How to Use This Template

- This file is the template AND the rulebook
- When the user pastes raw content, reshape it into the structure below
- Every section is bullet points only — no paragraphs, no prose

**Mandatory sections (every note must have these):**
- Elevator Pitch
- Definition
- Memory Hook

**Optional sections (include only when they apply):**
- ASCII Flowchart -> only if the concept has a flow or sequence
- Layered Diagram -> only for architecture-level topics
- Weak Area Clarification -> only when there is a confusing edge

**Hard rules (do not break):**
- No emojis anywhere — use `->`, `OK`, `BAD`, `[ x ]` instead
- Every section is short bullet points, one idea per line
- No paragraphs, no long sentences
- Elevator Pitch stays as one full sentence (the only exception)
- Real-World Analogy is required — short bullets, visual
- Code examples must have line-by-line plain-English comments
- Build code in small blocks, end with one Full Example block
- ASCII flowcharts go inside ```` ```text ```` blocks
- All headings use `######` (h6), separated by `---`
- Define every technical word inline in plain language
- Every note MUST include a Syntax section — the exact annotations and signatures a developer types, with no fluff

**Wiki-link rules (keeps the graph readable):**
- Each topic folder has ONE hub/overview note (e.g. `MVI - Overview`)
- Hub note -> links to ALL its children
- Child notes -> link UP to the hub PLUS at most 2 direct collaborators
- Never list every sibling in Related — that creates a "spaghetti graph"
- No inline `[[wiki-links]]` inside body sections; keep all links in Related
- Exception: the hub note may use inline links since its job is to point at everything

**Breadcrumbs frontmatter (mandatory for every non-hub note):**
- Every note in a topic folder must start with this YAML block at the very top:
  ```yaml
  ---
  up: "[[Hub Note Name]]"
  ---
  ```
- The hub itself (e.g. `MVI - Overview`) has NO frontmatter — it is the root
- A nested sub-page points UP to its parent page, not to the hub (e.g. `Task Manager - Full Code` points up to `Task Manager Screen - Exercise`)
- This is what makes the graph render as a tree

**Self-check before saving:**
- [ ] `up::` frontmatter present (unless this note is a root hub)
- [ ] Elevator Pitch present and is one sentence
- [ ] Definition is one line
- [ ] Real-World Analogy is present
- [ ] Every section is bullets, not paragraphs
- [ ] Syntax section present — exact annotations + signatures
- [ ] Code has line-by-line comments
- [ ] Memory Hook present and short
- [ ] No emojis
- [ ] Wiki-links in Related

---

```yaml
---
up: "[[Hub Note Name]]"
---
```

###### Elevator Pitch
- <One full sentence you would say in an interview>

---

###### Definition
- <One-line formal definition>

---

###### Real-World Analogy
- <Compare to daily life: restaurant, postman, recipe, etc.>
- <Make it visual, max 2-3 short bullets>

---

###### What
- <Bullet 1>
- <Bullet 2>
- <Bullet 3>

---

###### Why
- <Benefit 1>
- <Benefit 2>
- <Benefit 3>

---

###### Core Concepts
- <Concept 1> -> <one-line plain meaning>
- <Concept 2> -> <one-line plain meaning>
- <Concept 3> -> <one-line plain meaning>
- Link: [[]], [[]]

---

###### How it Works
- <Step 1>
- <Step 2>
- <Step 3>
- <Step 4>

---

###### Syntax
- <annotation or keyword> -> <what it does in one line>
- <annotation or keyword> -> <what it does in one line>

```kotlin
// Minimal working skeleton — just the required syntax, nothing extra
```

---

###### ASCII Flowchart *(optional)*

```text
[ Box A ]
    |
    v
[ Box B ]
    |
    v
[ Box C ]
```

---

###### Layered Diagram *(optional)*

```text
+-------------------+
|  Layer 1          |
+-------------------+
|  Layer 2          |
+-------------------+
|  Layer 3          |
+-------------------+
```

---

###### Example (Step-by-step)

**Step 1 — <what this step does>**

```kotlin
// Plain-English comment for this line
// Another plain-English comment
data class UiState(
    val items: List<String> = emptyList(), // visible list
    val isLoading: Boolean = false          // spinner flag
)
```

- <One-line takeaway from Step 1>

**Step 2 — <next step>**

```kotlin
// '.copy()' makes a new object with the changed field
_state.update { current ->
    current.copy(items = current.items + "New Item")
}
```

- <One-line takeaway from Step 2>

---

###### Full Example

```kotlin
// =========================================================
// One annotated block, top to bottom
// =========================================================
data class UiState(
    val items: List<String> = emptyList(), // visible list
    val isLoading: Boolean = false,        // spinner flag
    val error: String? = null              // error or null
)

class MyViewModel : ViewModel() {
    private val _state = MutableStateFlow(UiState())     // private holder
    val state: StateFlow<UiState> = _state.asStateFlow() // public read-only

    fun addItem(item: String) {
        _state.update { current ->
            current.copy(items = current.items + item)   // new state, no mutation
        }
    }
}
```

---

###### Common Mistakes
- BAD: <mistake 1>
- BAD: <mistake 2>
- BAD: <mistake 3>

---

###### Weak Area Clarification *(optional)*
- <Where beginners trip up>
- <Why it confuses>
- <One-line resolution>

---

###### Common Follow-up Traps
- Q: <follow-up question>
  A: <one-line answer>
- Q: <follow-up question>
  A: <one-line answer>
- Q: <follow-up question>
  A: <one-line answer>

---

###### Memory Hook
- <Short mnemonic or vivid phrase>

---

###### Key Rule
- <One-line takeaway>

---

###### Related
- wiki links 