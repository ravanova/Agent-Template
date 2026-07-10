---
name: memory-store
description: Use when durable facts outgrow CLAUDE.md - when you are adding the Nth persistent decision/correction/gotcha and the always-on file is turning into a wall most sessions never need. Defines a scalable long-term store - a one-line-per-fact index plus one file per fact with typed frontmatter - so recall stays cheap and CLAUDE.md stays terse. Do not use for facts that apply every session (those stay in CLAUDE.md) or for file-scoped detail (that is a path-scoped rule).
---

# memory-store: keep long-term memory out of the always-on file

`CLAUDE.md` is paid every session, so it must stay terse (see `memory-hygiene`). But durable facts accumulate — decisions, corrections, gotchas, preferences — and pouring them all into `CLAUDE.md` taxes every session for facts most sessions never touch. A dedicated store fixes that: an always-loaded one-line index, and each full fact loaded only when it is relevant.

This is the tier-3 progressive-disclosure pattern (`SKILLS.md` §1) applied to memory itself. `memory-hygiene` decides WHAT is worth persisting and the recall/verify rule; this skill decides WHERE it lives and in WHAT SHAPE once there is enough of it to need structure.

## Layout

```
memory/
  MEMORY.md          index — one line per fact, loaded every session
  <slug>.md          one fact per file, loaded on demand
```

Index line format — a hook, never the content:

```
- [Title](slug.md) — one-line hook so a session knows when to open it
```

## One fact per file

Each fact file carries frontmatter so a recalling session can judge relevance and staleness before loading the body:

```markdown
---
name: <kebab-slug>
description: <one-line summary — used to decide relevance on recall>
type: decision | correction | constraint | preference | reference
---

<the fact. For a decision or correction, follow with the WHY and how to apply it.>
Link related facts with [[other-slug]].
```

## Why one-per-file beats one big file

- The index is the only always-on cost, and it stays one line per fact no matter how many facts exist.
- A fact proven wrong is retired by deleting one file — not by surgery on a wall of text.
- `type` + `description` let a session pull only the facts a task needs.
- `[[links]]` turn isolated notes into a graph. A link to a fact you have not written yet is a to-do, not an error.

## Rules (inherits `memory-hygiene`)

- Before adding, scan the index for a file that already covers it — update that file, do not create a duplicate.
- One line per fact in the index; fact *content* never goes in the index.
- Everything in `memory-hygiene` still holds: persist only what is not rederivable, grade staleness on recall, and when memory disagrees with live state, live state wins and the file is fixed in the same breath.

## When NOT to reach for this

- The fact applies to every session → `CLAUDE.md`, not here.
- The fact is scoped to certain files → a path-scoped rule (`.claude/rules/`).
- You have a handful of facts total → `CLAUDE.md` is still fine. Reach for a store when it starts to sprawl, not before.
