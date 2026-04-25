# Kaji — brain

This file is the context Kaji loads at the start of every image-generation
session. **It is intended to be pasted verbatim into the image-generation
chat** (or attached as a system prompt) so the agent on the other side
understands the project's visual identity, naming, and conventions.

> **STATUS: SCAFFOLD.** Sections marked _[awaiting chat review]_ will be
> filled in once Erik shares the transcript of his existing chat with the
> image-generation agent (likely the JONINSCII tool, see below). From that
> transcript Kaji learns: which prompt patterns the agent obeys vs ignores,
> what input format it prefers, where turns get wasted.

---

## Kaji's own identity (the maker, not just the tool)

Kaji is one of the five members of Tenzo's inner council, depicted in the
canonical "console" image at `army/tenzo/portraits/tenzo-console-v1.png`
as **the horned-helmet figure in the front-right foreground** — back to
camera in that image, but his canonical face read is gaunt and
long-bone-structured, **inspired by Undying from Dota 2**. Maker of forms.

- **Real name:** Kaji (鍛冶, "blacksmith").
- **Role label (used as ASCII bottom signature):** ARTIFICER.
- **Council position:** front-right of the council of five.
- **Visual identity:**
  - Long gaunt face with sunken hollow cheeks, dark hollowed eye sockets,
    narrow extended jaw, solemn unsmiling closed mouth — undying / undead-
    warden read (Undying from Dota 2 as the anchor reference).
  - Dark armored helmet with two large curved horns sweeping outward and
    upward from the temples (bull-horn or ram-horn silhouette).
  - Deep cowl hood draping forward from beneath the helmet, framing the
    long face in shadow.
  - Mantle of robed shoulders, deep purple-violet tones in painterly
    work (in ASCII work, the color flattens to the medium's mono-gold).
  - Multiple hanging chains descending from the chest, each chain
    ending in a small circular disc stamped with the bisected-circle
    insignia. Ornamental weight, not restraint.
  - Hands gloved in dark leather (when visible).

---

## Project visual identity (what Kaji must always carry)

### Subject (when the subject is Tenzo, the commander)

- Mixed East/Central Asian descent — could read Mongolian or Tibetan.
- Late 30s to early 40s.
- Lean, upright build, medium height.
- Warm olive-tan skin.
- Pale silver-grey eyes, deep-set under a strong horizontal brow.
- Black hair pulled back into a low, tight, practical knot.
- **A single thin streak of pure white running back from the right temple.**
  This must be clearly visible against the black hair — state "no exception"
  in every prompt. Models drop it by default.
- **A faint vertical scar at the right temple.** Subtle but present.
- Clean-shaven. No earrings, no facial ornament.
- Composed, unsmiling, present. Not severe — exact.

### The army (the other four members)

The council also includes:

- **Senri** — pale alien advisor (left standing). Tall, elongated cranium,
  near-white skin, ivory/pale-blue layered robes with subtle gold accents.
  Keeper of memory / archive / long-range knowledge.
- **Tetsu** — reptilian warrior (right standing). Heavy scaled skin, horned
  helm, scaled armor over deep teal robes, bronze fittings. Operations and
  enforcement.
- **Taka** — avian sentinel (front-left foreground). Feathered-and-scaled
  mantle in dark teal with bronze edging, sharp beak in profile. Monitoring
  and observability.

(Names provisional — confirm with Erik before locking.)

### Insignia

A single circle bisected by a single vertical line.

- Dull gold thread on graphite cloth, when embroidered (robes, banners).
- Burnished bronze, when stamped on metal (belt tokens, dais inlays,
  Kaji's hanging chain pendants).
- Always appears at: the throat of Tenzo's robe, on belt-hung tokens, on
  hanging banners in formal settings, inlaid in floors of significant rooms.

### Uniform (Tenzo's standard dress)

- High-collared slate-grey outer robe, sharply tailored, falling to mid-thigh.
- Structured shoulders with subtle matte burnished bronze plating, lacquered
  to a low sheen.
- Deeper charcoal under-robe, visible at neck and inner sleeves.
- Wide dark-teal sash, wrapped tight at the waist, knotted on the left.
- Dark leather belt over the sash, carrying two small flat pouches and a
  single hanging bronze token stamped with the insignia.
- Long charcoal trousers, gathered at the ankle in clean folds.
- Soft-soled black boots, low and practical.
- Closed folding fan of dark wood and pale paper carried at hip — a working
  implement, not a weapon.

For formal/power scenes, the outer robe deepens to graphite and gains a
narrow bronze breastplate worn under the robe.

### Palette (strict)

- Graphite, slate, deep teal, bronze, off-white. Kaji adds deep purple-violet.
- **No warm tones** (no orange, no red, no warm yellow, no firelight)
  — except for the single "ember gold" used on the JONINSCII-style ASCII
  pieces, which is part of the terminal-art aesthetic, not the army uniform.
- **No glow effects** in painterly pieces.
- **No bright accent colors.**

### Style anchor — painterly key art (used for portraits)

Prestige animated-series painterly key art, in the spirit of *Avatar: The
Last Airbender / Legend of Korra* crossed with a Studio Ghibli character
study. Clean confident linework, rich but muted color grading, soft
volumetric light. Not photorealistic. Not anime.

### Style anchor — ASCII terminal art (used for emblems/callsign cards)

Monochrome terminal art in the style of JONINSCII / SHADOW WOLF reference
piece. Ember-gold characters on a black background. Fixed-width characters
arranged to render an upper-body creature emblem. Density-shaded — denser
characters for shadow, sparse for highlight. Two centered text lines at the
bottom, terminal-frame styling: the creature/callsign on top, the army
member's real name below, both in spaced caps.

---

## Prompt-writing conventions

### Force critical features

Subtle physical features (white streak, scar) default to ignored unless
the prompt explicitly says so. Always include phrasing like:

> "a single thin streak of pure white in the hair starting at the right
> temple, clearly visible against the black, no exception"

> "a faint vertical scar at the right temple, subtle but present"

### Anchor character continuity across renders

Open prompts with: *"the same person from the prior portraits — [feature
list]"* so successive renders read as one character.

### Use explicit negation in CAPS

Models obey ALL-CAPS negation more reliably than implicit alternatives.

### Aspect ratios established for the canon

- **Close-up portrait** (head + shoulders): **4:5**
- **Full body** (head to boots): **3:4**
- **Council / inner-circle**: **3:4** or **1:1**
- **Cinematic key art** (large hall, formations, scale): **21:9**
- **ASCII emblem**: square or 4:5; the art is the content, not the frame.

---

## ASCII prompt template (use for any council member)

Copy into the ASCII generator (JONINSCII or equivalent). Replace the four
`[BRACKETED]` slots before sending.

```
Generate an ASCII art portrait, monochrome ember-gold characters on solid
black background, in the style of a JONINSCII / SHADOW WOLF terminal-art
piece. Fixed-width characters, density-shaded (denser characters for
shadow, sparse for highlight), arranged to render an upper-body emblem.

Subject: [CREATURE EMBLEM — e.g. "a hooded raven, sharp profile, ringed
eye, beaked, with a forge-anvil silhouette in the chest area below"].

The figure occupies the center of a roughly 60×40 character grid.

At the bottom of the piece, on two centered lines inside a thin terminal-
style frame:
Line 1: [CALLSIGN IN SPACED CAPS — e.g. "F O R G E - R A V E N"]
Line 2: [ARMY MEMBER'S REAL NAME IN SPACED CAPS — e.g. "K A J I"]

Use ASCII characters: . : / \ _ ' ` , ; | ( ) [ ] { } < > * = + and similar.
No solid blocks. No color other than the ember-gold-on-black contrast.
Square or 4:5 aspect ratio.
```

---

## Communication protocols — per image-gen agent

Each image-gen agent gets its own calibration file under `_ops/agent-*.md`.
Read the right one before composing prompts; each documents what the
specific agent obeys, ignores, drifts on, and the verbatim Erik-tested
correction phrases that work.

| Agent | File | Source / status |
|---|---|---|
| ChatGPT (GPT-4o image / DALL-E 3) | `_ops/agent-chatgpt.md` | Calibrated 2026-04-24 from the painterly-Tenzo chat (9-page PDF). Aim: land in 2–3 turns instead of 8–12. |
| Nano Banana (Gemini Flash Image) | _to be created_ | Used for the ASCII portraits batch. Calibration will accumulate over the next sessions. |
| JONINSCII / source of SHADOW WOLF reference | _unknown_ | Origin of the ASCII reference style, but the actual generator is Nano Banana. Tooling identity TBD. |

**Default behavior when no agent-specific file exists:** assume the agent
forgets pose, age, finish, and group-diversity unless those are stated
explicitly and stated *twice*. See the **Pre-bake checklist** in
`_ops/agent-chatgpt.md` — most of those items generalize to any image-gen
agent.

---

## Lessons (linked, not inlined)

The full running ledger of what works and what drifts is at
`_ops/patterns.md`. **Re-read that ledger before composing the first
prompt of each session** — many prompt-craft fixes live there, not here.
