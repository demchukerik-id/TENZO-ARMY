# Swarm — Tenzo's subagents

The swarm is the set of internal subagents Tenzo spawns within each
Claude Code session via the Agent tool. Distinct from **the army** (named
operators that map to external services like ChatGPT image gen).

The swarm is **stateless across sessions** — Tenzo respawns them as
needed, and they read from the shared `_ops/` ledgers to inherit lessons
from prior sessions.

## Roster (6 members)

| Name | Role label | Function | Underlying tool |
|---|---|---|---|
| **Sora** | SCOUT | Internal research — codebase, files, fix docs, git history | `Agent: Explore` |
| **Sato** | SAGE | External research — web docs, API references, library lookups | `Agent: general-purpose` + WebSearch/WebFetch |
| **Goro** | RUNNER | Execution — Bash chains, deploys, dangerous ops handled carefully | `Agent: general-purpose` |
| **Mino** | WATCHER | Monitoring — logs, state, dashboards, often run_in_background | `Agent: general-purpose` (background) |
| **Fude** | SCRIBE | Ledger maintenance — appends scorecard, updates patterns | `Agent: general-purpose` (lightweight) |
| **Soji** | CURATOR | File organization — moves superseded files to `_trash/` per session | `Agent: general-purpose` (lightweight) |

2 research + 1 execution + 1 monitor + 1 scribe + 1 curator = 6.

## Files in this folder

- `INDEX.md` — this file (entry point)
- `ascii-prompts.md` — paste-able prompts for the 6 swarm portraits
- `portraits/` — saved swarm portrait renders (`<name>-ascii-v1.png`)
- `_ops/patterns.md` — accumulated lessons specific to swarm work

## Visual identity

The swarm is deliberately the visual opposite of the army. Where army
members are individual creatures (alien, reptile, raven, undying, human
commander), swarm members are **uniformed hooded silhouettes**, each
identifiable only by a single flat geometric mask glyph at the face
center. Same posture, same robe, same silhouette across all six —
different mask = different role.

This visual choice mirrors what they are: functional helpers, not
protagonists.

## Curator (Soji) — trash policy

Soji moves obsolete files to `fixes/context/_trash/` to keep working
folders clean. Trash is **subfoldered per session**, with original paths
preserved so restoration is a simple `mv` back.

### Trash location and naming

```
fixes/context/_trash/NNN-DAY-YYYY-MM-DD/<original-relative-path>/<file>
```

Session number `NNN` matches the GitHub push branch number (so a
session's trash folder and its pushed branch always share an ID).

### What Soji trashes (judgment-based)

- **Superseded prompts** — old version replaced by a newer canonical
- **One-shot working files** — empty outputs from failed tool calls,
  temporary intermediate files used to derive something else
- **Stale notes** — observations that have been promoted to formal docs
  and no longer apply

### What Soji NEVER trashes

- Identity files (`identity.md`, `brain.md`)
- Operational ledgers (`scorecard.md`, `patterns.md`)
- Generated artifacts (images, exports, renders)
- Active reference documents
- Anything still being iterated on

### First Soji action (this session)

Moved `army/kaji/diagrams/system-diagram-v1-prompt.md` (superseded by
`system-suite-prompts.md`) to
`_trash/001-FRI-2026-04-24/army/kaji/diagrams/system-diagram-v1-prompt.md`.
