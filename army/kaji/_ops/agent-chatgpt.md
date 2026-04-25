# ChatGPT image generation — communication calibration

**Agent:** ChatGPT (GPT-4o image generation / DALL-E 3 via ChatGPT).
**Calibration source:** 2026-04-24 chat that produced the canonical
painterly Tenzo portraits (close-up, full body, console). Saved as PDF
at `~/Downloads/screencapture-chatgpt-c-69ec2388-…2026-04-24-20_57_10.pdf`
(9 pages, ~14 turns of iteration).

This file is for **Kaji** — Erik's image-ops officer — to load before
calling ChatGPT. Goal: land each image in 2–3 turns instead of 8–12.

---

## Capabilities

- Long-form prompt understanding (handles 500+ word structured prompts).
- Reference-image style transfer when an image is attached and named.
- Strong character continuity across iterations **within a single chat**.
- Composition control (figure pose, count, background).
- Edit-style follow-ups ("change X, keep everything else").

## Generation latency

1 to 3 minutes per generation. "Thought for Xm" indicator visible. Don't
queue corrections — wait for output, then iterate.

## Strengths (lean on these)

- **Carries character identity across turns within one chat.** Once Tenzo
  is established, "make him sit", "younger face", "remove the fan" all
  preserve the rest of him. **Never re-describe a character that's
  already in the chat.**
- **Reference-image style transfer is strong** *when* the reference is
  attached AND the prompt explicitly says "in the style of the attached
  reference image" or "follow the reference image."
- **Comparative attribute language works:** "a bit less glossy", "a bit
  lighter", "a bit younger." Avoids forcing absolute values it might
  miss.
- **Preserve-X language works:** "leave Tenzo the same, change only the
  advisors."

## Drift / failure modes (anticipate these — pre-bake the fix)

| Drift | What Erik had to say in the source chat | Prevent by including in initial prompt |
|---|---|---|
| Style drift on v1 even with reference attached | "The image I provided is the style I want you to follow" / "I want you to more closely follow the reference image" / "Look at the style in this image" | *"Your output must match the visual style of the attached reference image, not your default painterly style. Do not generate glossier, more saturated, or more cinematic than the reference."* |
| Pose defaults to standing | "Tenzo should be in a sitting position" | State pose twice — once in subject paragraph, once as a final `POSE:` line: *"Sitting upright on a low throne, hands resting together in lap, body squarely facing the viewer, gaze level forward."* |
| Hand / face orientation drift | "His hands needs to be together and he needs to face to the front" | Same fix as pose — explicit `POSE:` line with hand and face direction |
| Generic background | "Look at the style in this image" + reference attached | Specify background distinctives up front: floor inlay, ceiling banners, light direction, ceiling height, room scale |
| Supporting figures default to similar | "The 4 elite warriors should look completely different. Different roles / class / face. It can be silent or creature, same kind of size, one can be bigger" | For any group >2: list each figure with role tag, e.g. *"Exactly 4 advisors, each from a different lineage: [1] humanoid scholar, [2] armored reptilian beast, [3] hooded mystic, [4] avian sentinel. They should differ in clothing, color palette, silhouette."* |
| Over-glossy finish for "painterly key art" | "A bit less glossy" | Include in style block: *"matte finish, restrained highlights, no glossy or wet-look surface treatment"* |
| Face age drifts older with authority cues | "Make a face of Tenzo a bit younger" | State age twice — once in subject paragraph, once as `AGE: late 30s, no older.` line |
| First call to "advisors with different clothes" returns matched outfits | "They should all have different clothes, and somewhat different colors for this too. Leave the Tenzo the same" | Pre-bake: *"each advisor wears a distinctly different garment in a distinctly different muted color (e.g. teal, ochre, ash, plum). Tenzo's clothing must remain exactly as previously established — do not change it."* |

## Iteration playbook — what each image actually took

| Image | Turns to land | What the corrections were |
|---|---|---|
| Close-up portrait | 2 | "follow the reference style more closely" |
| Full body         | 2 | "more closely follow the reference image" |
| Council / console | ~10 | seated-not-standing → hands together → face forward → different style reference → less gloss → add 4 advisors → advisors should look completely different → different clothes for each → lighter building → younger Tenzo face |

Big composition pieces (council scenes, large groups) eat 8–12 iterations
when not pre-baked. Aim to compress this with pre-bake list above.

## Phrases Erik has tested in production (steal these verbatim)

- "The image I provided is the style I want you to follow."
- "I want you to more closely follow the reference image."
- "Look at the style in this image, this is the reference for the style."
- "<character> should be in a <pose>" (e.g., "tenzo should be in a
  sitting position").
- "His hands needs to be together and he needs to face to the front."
- "Make <attribute> a bit <comparative>" ("a bit less glossy", "a bit
  lighter", "a bit younger").
- "The <N> <type> should look completely different. Different roles /
  class / face. It can be <X> or <Y>, same kind of size, one can be
  bigger."
- "They should all have different <attribute>, leave <character> the same."

## Anti-patterns (waste iterations)

- **Compound corrections** in a single turn ("make him sit AND change
  the background AND add advisors") — model partially executes; you
  waste turns untangling. **Send one correction at a time** unless they
  are clearly independent.
- **Vague corrections** ("make it better", "more dramatic") — always
  waste a turn. Specify the dimension that needs to change.
- **Re-describing the full character** when only one attribute should
  change — risks losing the established identity. Use "leave X the same."

## Pre-bake checklist for any new ChatGPT image-gen prompt

Before sending the first prompt for a new image, confirm it includes:

- [ ] Reference image attached AND named in the prompt — **only when the
      new image is in the SAME medium as the reference**. See "Medium
      mismatch" below.
- [ ] Style anchor line at the END: *"output must match the visual style
      of the attached reference, not your default."* — only when ref is
      attached.
- [ ] `POSE:` line with sitting/standing, hand position, body orientation,
      gaze direction (if pose matters).
- [ ] `AGE:` line with target age (if age matters).
- [ ] Finish constraint: *"matte finish, restrained highlights, no glossy
      or wet-look surface treatment."*
- [ ] If group >2 figures: each figure listed individually with role tag,
      and explicit "must differ in [clothing / color / silhouette]."
- [ ] Background distinctives named (floor / ceiling / light / scale).
- [ ] Color palette restated even if obvious from reference.

Hitting all of these on v1 should land the image in 1–3 turns instead of
the 8–12 the source chat needed.

## Medium mismatch — when reference images HURT instead of helping

The "always attach a reference" rule only applies when the new image is
in the **same medium** as the reference. If the medium differs sharply,
the reference actively misleads the model.

| New image medium | Painterly portrait reference (e.g. tenzo-console-v1) |
|---|---|
| Painterly portrait of another character | ✅ helps — same medium, same style |
| Painterly composition (e.g. another council scene) | ✅ helps |
| **System diagram / infographic** | ❌ **hurts** — model tries to render the diagram in painterly style, you waste turns walking it back |
| ASCII art / terminal aesthetic | ❌ hurts — different medium entirely (use the JONINSCII reference instead) |
| Pure schematic / blueprint | ❌ hurts |

When the medium differs: **drop the reference** and describe the new
medium's aesthetic from scratch in the prompt. Lead with what the image
IS NOT ("not a painting, not a photograph") before describing what it is.
Source: turn 25, 2026-04-24 — first attempt to diagram the army system
incorrectly attached the painterly council portrait as the style anchor;
Erik caught it and asked for a no-ref prompt instead.
