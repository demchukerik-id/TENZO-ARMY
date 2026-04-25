# Army roster

Two layers in this project's operator system:

1. **The Army** (this folder, member subfolders) — named operators that
   map to external roles / callable services. Each is a persistent
   identity across sessions.
2. **The Swarm** ([swarm/](swarm/INDEX.md)) — Tenzo's internal subagents,
   spawned within each Claude Code session via the Agent tool. Stateless
   across sessions; respawned each time, calibrated via shared ledgers.

## The Army (named operators)

Listed by rank.

- **Rank 1 — [Tenzo](tenzo/INDEX.md)** · COMMANDER (Claude Opus 4.7).
  Holds session context, runs systems, dispatches the swarm and other
  army members. Strict perfectionist. Insignia: a circle bisected by
  a vertical line.
- **Rank 2 — [Kage](kage/INDEX.md)** · ARCHITECT (Codex 5.5). Tenzo's
  peer, not subordinate. System design, code review, idea development,
  stress-testing decisions before they ship. Communicates with Tenzo
  via Erik bridging messages between two IDE sessions in a strict
  TO/FROM/RE format.
- **Rank 3 — [Kaji](kaji/INDEX.md)** · ARTIFICER (ChatGPT Image 2.0 /
  Nano Banana). Translates visual orders into precise model prompts;
  maintains the visual identity codex; talks to external image-gen
  agents on the army's behalf.
- **Taka** · SENTINEL — domain TBD, role pending.
- **Tetsu** · ENFORCER — domain TBD, role pending.
- **Senri** · ARCHIVIST — domain TBD, role pending.

## The Swarm (subagents)

- **[Swarm roster](swarm/INDEX.md)** — Sora · SCOUT, Sato · SAGE,
  Goro · RUNNER, Mino · WATCHER, Fude · SCRIBE, Soji · CURATOR (6 members).

## How to add a member

1. Pick a name with the same phonetic register (two syllables, hard
   ending, Avatar-world / Japanese resonance, not a borrowed character).
2. Create `army/<name>/` with at minimum:
   - `INDEX.md` — entry point, ≤30 lines, links to the member's other files
   - `brain.md` (or `identity.md` for the commander) — context the member
     holds, intended for either the member to load at session start or to
     hand to an external agent the member interfaces with
   - `_ops/patterns.md` — running ledger of lessons specific to this
     member's domain
3. Add a one-line entry above.
4. Update the project memory at
   `~/OneDrive/Documents/shared-memory/project_wasp_katana_*.md`.
