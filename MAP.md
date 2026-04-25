# Map of `fixes/context/`

Annotated index of every file in the context folder. Updated at every
session push. A fresh agent should be able to scan this and know which
file to open for any question.

---

## Top-level (`fixes/context/`)

- `2026-04-23-to-04-24-session.md` — original multi-day session log capturing the F5/MO/PO/ST/SA fixes 2026-04-23 → 2026-04-24. **Read once for historical context.** This is the source-of-truth on what was deployed before the army system existed.
- `HANDOFF.md` — last 2 session summaries + open items + pinned questions. **Read first as a fresh agent.**
- `STATUS.md` — current production state of the wasp-katana sync.
- `MAP.md` — this file.
- `_trash/` — superseded files moved here by Soji (CURATOR), subfoldered per session.

## Trash (`fixes/context/_trash/`)

Per-session subfolders preserve original paths so restoration is `mv` back. Naming: `NNN-DAY-YYYY-MM-DD/<original-relative-path>/<file>`.

- `_trash/001-FRI-2026-04-24/army/kaji/diagrams/system-diagram-v1-prompt.md` — first cleanup. The v1 system-diagram prompt was superseded by `system-suite-prompts.md`.

## Army (`fixes/context/army/`)

The two-layer operator system. Top-level resources:

- `INDEX.md` — Army roster + Swarm pointer (entry point for the operator system).
- `ascii-prompts.md` — paste-able prompts for the 5 army emblems (Tenzo / Kaji / Senri / Tetsu / Taka), Nano Banana format with SHADOW WOLF reference.

### Tenzo (`army/tenzo/`) — commander, Opus 4.7

- `INDEX.md` — Tenzo's protocols (auto-loaded behavior guide).
- `identity.md` — name origin (典座, Zen monastery cook), biography, insignia, painterly face prompts (close-up, full body, council).
- `prompts.md` — paste-able templates (browser agent, image model).
- `_ops/scorecard.md` — turn-by-turn scoring (+10 / +2). Updated after every turn.
- `_ops/patterns.md` — Tenzo's accumulated lessons (read before substantial action).
- `portraits/`
  - `tenzo-fullbody-v1.png` — canonical full body painterly portrait
  - `tenzo-console-v1.png` — canonical council/throne painterly portrait (used as palette anchor for in-medium painterly work)
  - `tenzo-ascii-v1.png` — JONINSCII-style emblem
  - `README.md` — naming convention

### Kaji (`army/kaji/`) — image-ops officer, ChatGPT Image 2.0 / Nano Banana

- `INDEX.md` — Kaji's role and workflow.
- `brain.md` — visual identity codex (paste verbatim into image-gen chats — visual anchors, palette rules, character continuity, prompt conventions).
- `portraits/`
  - `kaji-ascii-v2.png` — canonical horned-undying ASCII emblem
  - `README.md` — naming convention
- `diagrams/` — system diagrams Kaji has produced for the army:
  - `system-suite-prompts.md` — 5 paste-able prompts for the system-explanation suite
  - `system-suite-01-architecture.png` — done
  - `system-suite-04-dispatch-push.png` — done
  - `system-suite-05-onboarding.png` — done
  - (system-suite-02 and 03 still pending)
- `_ops/agent-chatgpt.md` — ChatGPT image-gen calibration (capabilities, drift table, pre-bake checklist, medium-mismatch rule). **Read before composing any ChatGPT prompt.**
- `_ops/patterns.md` — image-gen lessons (cross-agent).

### Other army members (placeholder folders, identity TBD)

- `army/senri/portraits/` — Senri's ASCII portrait (`senri-ascii-v1.png`).
- `army/tetsu/portraits/` — Tetsu's ASCII portrait (`tetsu-ascii-v1.png`).
- `army/taka/portraits/` — Taka's wrong-labels reference (`_taka-ascii-v0-wronglabels.png`); regen pending.

These don't yet have `INDEX.md` / `identity.md` / `_ops/` scaffolding — awaiting Erik to define their roles.

### Swarm (`army/swarm/`) — Tenzo's 6 subagents (internal helpers)

- `INDEX.md` — Swarm roster (Sora / Sato / Goro / Mino / Fude / Soji) + trash policy.
- `ascii-prompts.md` — 6 paste-able front-view prompts for the swarm portraits.
- `portraits/` — empty, awaiting Nano Banana renders.
- `_ops/patterns.md` — swarm dispatch lessons (scaffold; per-member calibration table inside).

---

## What's NOT in this map

- `fixes/active/`, `fixes/solved/`, `fixes/F5/`, `fixes/MO/`, etc. — those are project work documents (per-flow fix docs), not Tenzo's self-tracking. Use `STATUS.md` to navigate them.
- `~/.claude/projects/<key>/*.jsonl` — raw chat transcripts (live during sessions; copied into `_trash` or `sessions/` at session push, depending on the push procedure once it's built).
- `~/OneDrive/Documents/shared-memory/` — shared memory store (`MEMORY.md` index + `project_wasp_katana_*.md` entries). Auto-loaded by Claude Code.
