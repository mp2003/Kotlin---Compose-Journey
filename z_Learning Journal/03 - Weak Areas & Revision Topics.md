###### How to Use
- Pick any topic marked [ ] and ask Claude: "quiz me on this" or "explain this the way I learn"
- Mark [x] when it feels solid after revision
- Add new weak areas here as they come up in sessions

---

###### Room

- [ ] Singleton pattern — double-checked locking
  - why @Volatile on the instance field
  - why check `instance ?:` twice (outside AND inside synchronized)
  - what synchronized(this) actually prevents
  - reference: [[Room/03 - Room - Database]]

- [ ] @Database annotation parameters
  - entities = [...] — why the label is mandatory (not positional)
  - version — when to bump it
  - exportSchema — what it does and why false for learning
  - reference: [[Room/03 - Room - Database]]

- [ ] Flow-from-DB mental model
  - why reads return Flow but writes are suspend
  - how a table change triggers a Flow emission
  - how stateIn() converts a Flow to a StateFlow for the ViewModel
  - reference: [[Room/05 - Room - Flow from Database]]

---

###### Kotlin Patterns

- [ ] .also { } — what it does, when to use it vs. .let / .apply
- [ ] Elvis operator ?: chained — reading `a ?: b ?: c` fluently
- [ ] companion object vs object — difference and when to use each

---

###### To Add (future sessions)
- CameraX — runtime permissions, lifecycle binding, use cases
- Jetpack Canvas — Path, DrawScope, custom Shape
- ViewModel factory pattern (for passing repository without Hilt)
