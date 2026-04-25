# Context tree diagram — prompt

For ChatGPT image-gen. **No reference image** (medium mismatch — we're
making a tree diagram, not a portrait). 1:1 square. Visualizes the
state of `fixes/context/` as of 2026-04-25.

When the render comes back, save to:
`army/kaji/diagrams/context-tree-v1.png`

```
A clean hierarchical tree diagram visualizing a folder structure, in
flat painterly-but-diagrammatic illustration style. NOT a painting, NOT
photographic, NOT 3D rendered. Think a high-end product architecture
diagram crossed with a stylized scroll-of-the-realm — the kind of map
that hangs on a war-room wall. 1:1 square composition.

TITLE at top in serif small caps, dull gold:
"FIXES/CONTEXT · TENZO ARMY MAP"

LAYOUT: a left-to-right hierarchy tree, root on the left, branches
expanding right. Generous spacing between nodes. Thin gold connecting
lines between parent and child nodes (clean L-shape branches).

ROOT NODE (far left, larger): a square label box with serif small caps:
"FIXES/CONTEXT"
Behind the root node: a faded large watermark of the army insignia (a
single circle bisected by one vertical line) in dull bronze.

LEVEL 1 BRANCHES from the root, stacked vertically:
  [1] "HANDOFF.md" — small file icon, gold tag "fresh-agent bridge"
  [2] "STATUS.md" — small file icon, gold tag "production state"
  [3] "MAP.md" — small file icon, gold tag "this map's source"
  [4] "_TRASH/" — small folder icon with a broom glyph beside it, gold
      tag "session-subfoldered cruft"
  [5] "ARMY/" — folder icon, gold tag "the operator system" — this
      branch is the largest and expands further to the right

LEVEL 2 EXPANSION from "ARMY/" — five member subfolders shown as small
circular portrait icons with names below:
  - TENZO (commander, larger circle than the others)
  - KAJI (with horned-helm hint inside the icon)
  - SENRI (with elongated alien skull hint)
  - TETSU (with reptilian/horned hint)
  - TAKA (with raven beak hint)
  - SWARM/ (rendered as a SINGLE square containing six tiny uniform-
    hooded silhouettes in a 3x2 grid, labeled "SORA · SATO · GORO ·
    MINO · FUDE · SOJI" below)

LEVEL 3 EXPANSION from each member (small text, expands only on the
right side of each member's circle):
  - Tenzo →  identity.md · INDEX.md · prompts.md · _ops/{scorecard,
    patterns} · portraits/{3 painterly + 1 ascii}
  - Kaji →  brain.md · INDEX.md · _ops/{agent-chatgpt, patterns} ·
    diagrams/{5 prompts, 3 renders} · portraits/
  - Senri →  portraits/ · (identity TBD) — shown faded
  - Tetsu →  portraits/ · (identity TBD) — shown faded
  - Taka →  portraits/ · (identity TBD) — shown faded
  - Swarm →  INDEX.md · ascii-prompts.md (6) · _ops/patterns.md ·
    portraits/ (empty, awaiting renders)

VISUAL HIERARCHY:
- Built-out / canonical members (TENZO, KAJI, SWARM) rendered in full
  ember-gold lines and full opacity
- Placeholder members (SENRI, TETSU, TAKA) rendered slightly faded
  (50% opacity) with a small dashed outline — visually signals "still
  to be developed"

LEGEND in the bottom-right corner, small serif gold:
- solid line = built and load-bearing
- dashed line = scaffold / development pending
- broom glyph = under Soji's curation (trashable when superseded)
- circle insignia = part of the army

PALETTE (strict): graphite (background), slate (mid-tones), deep teal
(accent on the SWARM box), dull bronze (frames, watermark, member
circles), off-white (highlights), thin gold (connection lines, labels).
NO warm tones. NO orange, NO red.

FINISH: matte finish, restrained highlights, no glossy or wet-look
surfaces, clean confident linework, generous negative space. Labels in
serif small caps, dull gold. The tree should feel calm and navigable,
not crowded.
```
