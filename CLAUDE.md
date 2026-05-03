# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **not a code project**. It is an **Obsidian vault** containing personal study notes for an 8-week Kotlin/Android/Jetpack Compose learning roadmap (target: June 2026). Each `.md` file is a study note; folders group notes by topic. There is no build system, no tests, no source code to compile — only Markdown.

The user is studying Android development and writes notes here as they learn. When asked to "add a note", "explain X", or "fill in Y", the deliverable is a new or edited Markdown file in this vault, not code.

## Repository structure (semantic, not exhaustive)

- `RoadMap.md` — the master 8-week plan with checkboxes tracking progress. Source of truth for what topic the user is currently on. Check this first to know what's in scope this week.
- `note Format.md` — the **canonical note template**. Every new study note must follow this structure exactly (see "Note format" below).
- `MVI Architecture/` — Week 1 notes (State, Intent, Effect, Reducer, ViewModel, onIntent, etc.) plus a Task Manager exercise.
- `DI/` — Week 2 notes on Dependency Injection / Hilt.
- Future weeks (Networking, Navigation, Room, Permissions, Hardware, Full App) will get their own folders as the user progresses.

Notes link to each other with Obsidian wiki-link syntax: `[[Note Name]]`. Preserve these — they form the graph view the user navigates by.

## Note format (authoritative source)

- Read `note Format.md` first — it is both the template and the rulebook
- The "How to Use This Template" section at the top is authoritative
- Do not re-derive rules from existing notes — most pre-date the current template
- Existing notes get reformatted on demand when the user pastes them

**Hard rules (do not break):**

- No emojis — use `->`, `OK`, `BAD`, `[ x ]` instead
- Every section is bullet points, NOT paragraphs
- One idea per bullet, short
- Elevator Pitch is the only full sentence allowed
- Three mandatory sections: Elevator Pitch, Definition, Memory Hook
- Real-World Analogy is required (short bullets)
- Code comments must be line-by-line, plain English
- Small code blocks during explanation, one Full Example at the end
- ASCII flowcharts only when there is a flow — inside ` ```text ` blocks
- All headings use `######` (h6), separated by `---`
- Define every technical word inline

**Wiki-link rules (keeps Obsidian graph readable):**

- Each folder has one hub/overview note (e.g. `MVI - Overview`, `DI - Overview`)
- Hub links to all its children; children link UP to the hub + at most 2 collaborators
- Never make every note link to every other note — that makes a spaghetti graph
- Keep all links in the Related section; no inline `[[wiki-links]]` in body sections
- The hub note is the only exception — it may use inline links

## User context (calibrate to this)

- User is a **beginner Android/Kotlin learner studying for interviews**
- Re-reads notes repeatedly to generate insights
- Visual learner — connects dots through analogies and flowcharts
- Long text creates phobia — keep everything as short bullet points
- Plain language always — define jargon inline
- Match existing depth of sibling notes — do not pad

## Working in the vault

- **Never invent content the user hasn't asked for.** These are the user's personal study notes — write what they request, in their voice (concise, bullet-driven, present tense). Don't add speculative sections or tangents.
- **Match the existing depth.** Some notes are 30 lines; some are 170. Look at sibling notes in the same folder before deciding length.
- **Filenames may contain special characters and spaces** (e.g. `Coding - Third-party DI (@Provides).md`). Always quote paths in shell commands.
- **Don't touch `.obsidian/`** unless explicitly asked — it holds Obsidian's workspace state, plugin config, and hotkeys, and is rewritten by the app.

## Git workflow

The `obsidian-git` community plugin auto-commits this vault on a timer with messages like `vault backup: <timestamp>`. Recent history is dominated by these auto-commits.

- Do **not** create commits for content edits unless the user explicitly asks — the auto-backup will pick them up.
- When the user does ask for a manual commit, write a real, descriptive Conventional Commits-style message (e.g. `docs(di): add @Provides vs @Binds note`) — do not mimic the `vault backup: ...` style, which is reserved for the plugin.
- The working tree often shows modifications to `.obsidian/workspace.json` and stray `.DS_Store` files. Ignore these unless the user is specifically asking about Obsidian config.

## Roadmap awareness

Before adding new notes, check `RoadMap.md` to see which week/topic the user is on (look for the first unchecked `[ ]` items). New notes should typically map to a roadmap bullet — if a request doesn't fit anywhere on the roadmap, ask the user where it belongs rather than guessing a folder.
