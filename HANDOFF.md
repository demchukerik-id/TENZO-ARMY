# Session handoff

This file holds summaries of the last 2 sessions, plus open items and
pinned questions waiting for Erik. Updated at every session push. A
fresh agent reading this should know exactly what's in flight and what's
blocked, in 2 minutes.

---

## Latest session — 2026-04-24 / 2026-04-25 (~30 turns)

**Theme:** Built the army + swarm operator system from scratch.

**What got built:**

- **Tenzo identity** — Mongolian/Tibetan-read commander, late 30s, strict
  perfectionist. Painterly portraits done (close-up, full body, council /
  console). Insignia: a circle bisected by a single vertical line.
- **The Army** (5 named operators, ASCII emblems for 4 of 5):
  - Tenzo · COMMANDER (Opus 4.7)
  - Kaji · ARTIFICER — image generation (ChatGPT Image 2.0 / Nano Banana)
  - Senri · ARCHIVIST (role TBD)
  - Tetsu · ENFORCER (role TBD)
  - Taka · SENTINEL (raven, not hawk — see open decisions)
- **The Swarm** (6 subagents, prompts ready, no portraits yet):
  - Sora · SCOUT — internal research (`Agent: Explore`)
  - Sato · SAGE — external research (web/docs)
  - Goro · RUNNER — execution (Bash, deploys)
  - Mino · WATCHER — monitoring (run_in_background)
  - Fude · SCRIBE — ledger maintenance
  - Soji · CURATOR — file organization, trash policy
- **Image-gen calibration** — read Erik's ChatGPT chat PDF, extracted
  pre-bake checklist that compresses 8–12 iterations to 2–3. Lives at
  `army/kaji/_ops/agent-chatgpt.md`.
- **System diagram suite** — 5 prompts written (`army/kaji/diagrams/system-suite-prompts.md`),
  3 of 5 rendered and saved (Architecture, Dispatch+Push, Onboarding).
- **Trash policy** — `fixes/context/_trash/NNN-DAY-DATE/<original-path>`
  for superseded files; first cleanup demonstrated.
- **Onboarding scaffolding** — this file plus `STATUS.md`, `MAP.md`,
  and `CLAUDE.md` at project root.

**Score for the session:** 71% (~214/300 across 30 turns). Trend: improving
late in the session as calibration paid off.

---

## Earlier sessions

_This is the first session created under the handoff format. Future
sessions slot in here, newest first, oldest pruned after 2 sessions._

---

## Open decisions waiting on Erik

1. **GitHub repo for session push** — recommended: same `KATANA-WASP-FINAL-DEBUG` (existing remote, contains `001-WED-…` / `002-THU-…` / `003_FRI_…` branches). Awaiting Erik confirmation.
2. **Push trigger phrase** — what Erik will say to initiate the push. Candidates: `"push context"` / `"wrap up"` / `"save session"`. Pick one stable phrase.
3. **Spawning style for the swarm** — lazy+sticky (recommended: spawn each subagent first time it's needed, keep alive for the session) vs `/wake` slash command (spawn all 6 at session start). Or both.
4. **Senri-vs-Kaji image-gen naming** — Erik once referred to "Senri" as the image-gen agent; the established agent is Kaji. Charitable reading is Erik meant Kaji. Confirm before next session uses the wrong name.
5. **Taka path for ASCII v2** — Option A (label-swap on the existing wrong-labels render in `taka/portraits/_taka-ascii-v0-wronglabels.png`) or Option B (full raven regen via the corrected prompt in `army/ascii-prompts.md` Section 5).
6. **TAKA vs KARASU naming** — "Taka" (鷹) means hawk in Japanese but the character is canonically a raven. Keep TAKA, or rename to KARASU (烏)?
7. **Anything in `fixes/context/` to keep private** — currently the plan is to push everything to GitHub; nothing in `army/` references PII or prod credentials, but worth confirming.

## In-flight work

- **Image renders pending** (8 total):
  - `system-suite-02-army-roster.png`
  - `system-suite-03-swarm-roster.png`
  - 6 swarm portraits (Sora / Sato / Goro / Mino / Fude / Soji)
- **First GitHub push** — pipeline designed and documented, not yet executed.
- **Live Fude spawn test** — offered, awaiting Erik's go-ahead.
- **Senri / Tetsu / Taka identities** — folder placeholders exist but `INDEX.md` / `identity.md` scaffolding not built; awaiting Erik to define their roles.
- **`fixes/` reorganization** — partial. The legacy `fixes/F5/`, `fixes/MO/`, `fixes/PO/`, `fixes/SA/`, `fixes/ST/` folders still hold solved docs that should move to `fixes/solved/<flow>/` per the 2026-04-23-to-04-24-session.md plan.

## Pinned questions for the next conversation start

Surface these to Erik in turn 1 of the next session if he doesn't pre-empt:
- Which open decision (1–7 above) does he want to resolve first?
- Wants to test Fude live, or proceed with image renders, or continue building infrastructure?
