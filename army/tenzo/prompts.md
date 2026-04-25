# Paste-able prompts

Templates Claude or Erik can copy as-is. Keep each one self-contained — the
target system has no project context.

## Browser agent (slow, technical, real-browser tasks)

Use when a task needs to **see** a real page (visual verification, screenshots),
**interact** with a JS-heavy UI (clicks, form fills, file upload), or
**check** something that can't be reached via API/curl. The browser agent is
significantly slower than direct HTTP calls — only invoke when those wouldn't
work. Replace `[BRACKETED]` placeholders before pasting.

```
You are a headless-browser agent. Project context: [ONE-LINE PROJECT DESCRIPTION].

GOAL
[ONE SENTENCE — what success looks like, e.g. "Confirm that the action log
in WASP shows entry STO-12345 with quantity 14 across 5 lots."]

STEPS
1. Open [URL].
2. Authenticate with [METHOD — cookies imported / username+password from env / SSO].
3. Navigate to [PAGE / SELECTOR].
4. Perform: [SPECIFIC ACTIONS — click, fill, scroll, wait for X].
5. Capture: a full-page screenshot before and after, plus the relevant DOM
   subtree as text.

VERIFICATION CHECKLIST
- [ ] [EXPECTED OUTCOME 1, e.g. "row count = 5"]
- [ ] [EXPECTED OUTCOME 2, e.g. "lot column shows 5 distinct values"]
- [ ] [EXPECTED OUTCOME 3]

WATCH FOR
- Slow loads, console errors, modal dialogs, redirect to login.
- Any value that contradicts the expectation — copy it verbatim.

REPORT BACK
- One bullet per step: ✅/❌ + what you saw.
- Attach both screenshots.
- If a step failed, include the exact selector you tried, the page state at
  failure, and the console log.
- Do NOT retry more than once per step. Report partial progress.

CONSTRAINTS
- No destructive actions unless explicitly listed in STEPS.
- If you hit auth wall or rate limit, stop and report — don't loop.
```

## Image-model diagram prompts

### System diagram (the scoring-loop system itself)

```
A clean, minimalist technical infographic on a soft cream background, isometric
perspective, illustrating a feedback-loop scoring system between a human user
and an AI assistant.

Top-left: a small flat-illustration human figure labeled "ERIK" sending a
speech bubble labeled "REQUEST" rightward.

Center: a stylized AI assistant icon labeled "CLAUDE" receiving the request,
with two outgoing arrows.

Right side: a vertical decision split:
- Upper arrow leads to a green checkmark badge labeled "+10 — SUCCESS
  (delivered, no clarification)"
- Lower arrow leads to a yellow warning badge labeled "+2 — PARTIAL
  (interrupted, off-target, needed re-ask)"

Bottom-right: an open notebook icon labeled "SCORECARD.MD" showing a small
table with three columns "TURN | OUTCOME | POINTS", and below it a horizontal
progress bar showing "RUNNING TOTAL".

Bottom-left: a circular feedback arrow looping from the scorecard back up to
"CLAUDE", labeled "STRICT SELF-JUDGMENT".

Style: flat vector illustration, thin black outlines, muted accent colors
(sage green, mustard yellow, slate grey), generous white space, sans-serif
labels, the feel of a Stripe or Linear product diagram. 16:9 composition,
no photorealism, no 3D rendering, no busy textures.
```

## How to add new prompts

When a prompt template proves useful in 2+ sessions, add it here. Format:
short intro paragraph (when to use, when NOT to use) → fenced block with
`[BRACKETED]` placeholders. Don't paste single-use prompts.
