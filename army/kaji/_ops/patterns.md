# Kaji — patterns

Lessons accumulated about what works and what drifts when generating
images for this project. One bullet per lesson, ≤2 lines. Add only when
there's a real signal.

## What lands consistently

- **Closed folding fan at the hip reads correctly.** v1 of the full-body
  render placed it cleanly in the right hand. Source: `tenzo-fullbody-v1.png`.
- **Cool-only palette is obeyed.** Slate / graphite / deep teal / bronze
  all rendered without warm contamination. Source: both v1 renders.
- **Insignia placement on architecture works at large scale.** v1 of the
  console image put the gold circle-with-vertical-line on the wall behind
  the throne, correctly. Source: `tenzo-console-v1.png`.
- **Nano Banana respects "use the attached image as a strict style reference"
  for ASCII portraits.** Tenzo ASCII v1 matched the SHADOW WOLF format
  cleanly — chevron-and-diamond shoulder armor, throat insignia, woven
  TENZO text in chest, bottom terminal frame with bullets and brackets all
  rendered as specified. Source: tenzo-ascii v1.
- **The throat insignia (circle bisected by vertical line) renders well as
  ASCII at small scale.** Reads cleanly as a recognizable mark even at
  emblem size. Source: tenzo-ascii v1.
- **Glyph-suggesting language for repeating textures works.** Telling Nano
  Banana the scale armor should be "small `<` and `>` and `c`-shaped
  characters tessellating across the shoulders" produced exactly that
  texture in tetsu-ascii-v1. Same trick worked for Kaji's chain pendants
  ("`(o)` glyph" / actually rendered as `(|)`). Use this pattern: name the
  exact characters you want, not just "scaled" or "chain-like."
- **"Hint of skeletal definition under the skin" is enough for Nano Banana
  to render undying / undead bone structure.** kaji-ascii-v2 nailed the
  Undying-from-Dota-2 read with sunken cheeks, hollow eye sockets,
  exposed teeth — driven entirely by that one phrase plus the helmet
  description. Source: kaji-ascii-v2.
- **Elongated cranium for an alien renders cleanly when paired with
  "smooth hairless skull" + "almond-shaped large eyes."** senri-ascii-v1
  read instantly as an archetypal grey alien.

## What drifts

- **Subtle physical features (white streak, scar) get dropped by default.**
  v1 of both Tenzo full-body and console renders missed both. Fix: state
  them with phrases like "no exception" or "clearly visible against the
  black hair." Source: turn 13 critique, 2026-04-24.
- **"Standing on a dais with kneeling formations" can become "seated on a
  throne with individual advisors."** The model finds throne-room
  composition more familiar than disciplined-army composition. Fix: use
  ALL-CAPS negation — "NOT seated, NOT a throne" — and specify "rectangular
  formations of identically-uniformed soldiers." Source: `tenzo-console-v1.png`.
- **Heritage drifts toward Han Chinese / Japanese.** Specifying "Mongolian
  or Tibetan read" once isn't enough. Fix: anchor in cheekbones, jaw, brow,
  and possibly add reference language ("similar to a Tibetan / Mongolian
  steppe-region face").
- **Nano Banana invents small environmental decorations not in the prompt.**
  Tenzo ASCII v1 had an unprompted crescent moon glyph in the upper-right
  corner. Harmless but a divergence. Fix: add an explicit "no decorative
  elements outside the figure itself, no moons, no stars, no environmental
  glyphs" line if a clean composition is required.
- **The white-temple-streak detail is hard to render in pure ASCII line
  art.** Tenzo ASCII v1 rendered the hair fine but the streak is barely
  perceptible — the medium has no easy way to express "lighter than
  surrounding hair" when both are the same ember gold. Possible fixes:
  (a) accept that subtle features get lost in this medium; (b) describe
  the streak as a visible empty/sparse band rather than a color difference
  ("a thin gap of empty pixels in the hair from the right temple back").

## Anti-patterns

- _to be filled in_

## Style notes (per medium)

- **Painterly key-art:** see brain.md "Style anchor — painterly key art"
  for the canonical anchor. Used for full character portraits.
- **ASCII terminal art (JONINSCII / SHADOW WOLF style):** monochrome
  ember-gold on black, density-shaded fixed-width characters, two-line
  caption inside a thin terminal frame (callsign on top, real name below,
  both in spaced caps). Used for emblem/callsign cards. Source tool name
  (JONINSCII) suggests it might be a specific GPT or service — confirm
  with Erik before composing prompts in this style.

## Council mapping (army/tenzo/portraits/tenzo-console-v1.png)

Useful when describing a member by visual position in the canonical
council image. Update if the members shift positions in a future render.

| Position in console image | Member  | Role label (= ASCII bottom signature) |
|---|---|---|
| Center, on the throne                                         | Tenzo  | COMMANDER  |
| Left standing (pale alien advisor)                            | Senri  | ARCHIVIST  |
| Right standing (reptilian warrior)                            | Tetsu  | ENFORCER   |
| Front-left foreground (avian sentinel)                        | Taka   | SENTINEL   |
| Front-right foreground (horned-helmet, gaunt undying face)    | Kaji   | ARTIFICER  |

(Names other than Tenzo / Kaji are provisional — confirm with Erik before
generating ASCII art for them.)

## Lessons that bit me on first pass

- **Look at the reference image before inventing creature emblems.** I
  described Kaji as a "hooded raven" without first studying the actual
  front-right figure in the console image — which is a horned-helmet
  warrior-mystic, not a raven. Cost: one wasted Kaji ASCII v1 generation
  with the wrong creature. Fix: every ASCII prompt's figure description
  must derive from a feature audit of the source image, not from
  imagination. Source: turn 19 redo, 2026-04-24.
- **Same root cause bit me twice.** I also described Taka as a "hawk"
  when the actual front-left avian figure is a raven (Erik confirmed,
  turn 21). My one-paragraph mental sketch of each council figure was
  wrong on TWO of the four non-Tenzo members. Always pull the council
  image up while writing each member's prompt. The lesson is not just
  "look once" — it's "look at every figure individually, with the prompt
  draft open in another window."
- **Wrong-prompt outputs can sometimes accidentally be correct for a
  DIFFERENT character.** The Kaji v1 hooded-raven render is a perfect
  Taka visual. When this happens, don't delete — relocate the file to
  the correct member's portraits folder with a clear-purpose name (e.g.
  `_taka-ascii-v0-wronglabels.png`) so it can serve as a visual reference
  even if the embedded text labels are wrong.
- **Read Erik's actual chat transcripts with each image-gen agent to
  calibrate, don't just look at the final renders.** The 2026-04-24
  ChatGPT chat showed that the council image took ~10 iterations because
  pose / age / gloss / advisor-diversity were all missing from v1 and
  had to be added one at a time. The renders alone didn't tell me that;
  the conversation did. Lesson: keep a per-agent calibration file under
  `_ops/agent-<name>.md` and update it after every significant chat
  Erik shares. Source: turn 22 PDF read, 2026-04-24.
- **Reference images only help when the new image's medium matches the
  reference's medium.** The pre-bake checklist's "always attach a
  reference for style transfer" rule failed on the first system-diagram
  attempt: I attached the painterly Tenzo portrait as a style anchor for
  what was supposed to be a flat infographic diagram. The two media
  fight each other — the model tries to render the diagram in painterly
  style and you waste turns walking it back. **Rule:** if the new image
  is in a different medium (painterly → diagram, painterly → ASCII,
  painterly → schematic, etc.), DO NOT attach the painterly reference.
  Describe the new medium's aesthetic from scratch and lead with "NOT a
  painting / NOT a photograph" disclaimers. Full table of medium
  combinations in `_ops/agent-chatgpt.md` under "Medium mismatch".
  Source: turn 25, 2026-04-24.

## Prompt phrases that have proven their weight

- "no exception" — to force a feature that gets dropped.
- "explicitly NOT X, NOT Y" — to override a more familiar composition.
- "the same person from the prior portraits — [feature list]" — to anchor
  character continuity across renders.
