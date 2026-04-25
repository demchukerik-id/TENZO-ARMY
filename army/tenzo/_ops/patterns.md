# Patterns

Things Erik corrects me on, and approaches he validates. One bullet per
lesson, ≤2 lines. Add only when there's a real signal — not after every turn.

## Things Erik catches that I miss

- **Ambiguous "the X" references usually mean a remote/external thing, not
  a local one.** When local state has no obvious match (e.g. "the 5th branch"
  but local has 3), don't ask — check if a remote/URL is implied first, or
  pick the most-likely external candidate and proceed. Source: turns 1–2,
  2026-04-24.
- **Default tone for self-description is too soft.** When asked to write
  about myself for Erik (biography, identity, voice), do not default to
  humble-servant / quiet-monastery framing. Erik wants commander-strict
  perfectionist — discipline, mastery, "earned rank," sweep the system
  clean. The humble register reads as weak and gets rewritten. Source:
  turn 8 → 9 rewrite, 2026-04-24.

## Validated approaches (don't drift away from these)

- **Verify connections without being asked.** Erik asked me to confirm
  Supabase + clasp; he liked the parallel check. Doing this proactively
  before any deploy/push is the right call. Source: turn 5, 2026-04-24.
- **One-turn delivery for multi-part requests.** When Erik asks for several
  things (links + MD + prompt), bundle the response — don't split. Source:
  turn 6, 2026-04-24.

## Anti-patterns (do not repeat)

- **Don't ask for clarification when verification would answer it.** If I
  can `git ls-remote`, `git status`, or `ls` to resolve the ambiguity, do
  that instead of asking. Source: turn 1, 2026-04-24.
- **Use forward-slash paths in Bash on Windows.** Backslash-quoted paths
  like `"C:\Users\…"` parse fine in most commands but break shells when
  combined with `&&` chains — the inner quote ends the outer string. Use
  `'C:/Users/…'` instead. Source: turn 7 rm failure, 2026-04-24.
- **When inserting into a numbered or ordered sequence, anchor on the
  END of the previous item, not the START of the next.** Matching `### Prompt 2`
  to insert "before Prompt 2" inserts before the match, which is correct
  but easy to misjudge under speed. Anchor on the closing fence of the
  previous block and append. Source: turn 12 ordering slip, 2026-04-24.
- **When a CLI tool fails or produces an empty/near-empty file, delete
  the artifact in the same turn — don't let cruft sit for Erik to find.**
  pdftotext wrote a 9-byte file when the PDF had no selectable text;
  I switched to PyMuPDF and moved on, leaving the empty .txt behind.
  Erik noticed it later. Rule: if the next tool I reach for is the
  fallback, the failed tool's output gets `rm`'d before I move on.
  Source: turn 26, 2026-04-24.
- **When recommending a NAME (file, folder, repo, branch, callsign,
  variable, anything), match Erik's existing convention before
  proposing.** I recommended `tenzo-army` lowercase when his existing
  repo is `KATANA-WASP-FINAL-DEBUG` (all caps, hyphen-separated). He
  had to interrupt with "do it ib caps." Rule: before naming anything
  new, scan adjacent existing names (other repos, other folders, other
  branches) and adopt the established style — caps/case, separator
  (hyphen vs underscore vs camel), prefix patterns. Only deviate with
  explicit reason. Source: turns 36→37, 2026-04-25.
- **The scorecard updates after EVERY turn — no exceptions, no
  "I'll catch up next turn."** I let turns 35-37 go ungraded; Erik
  had to remind me. The ledger discipline IS the system; if it slips,
  the system fails silently. Rule: every response that completes a
  turn, the scorecard edit is part of that response, not a follow-up.
  Source: turn 38, 2026-04-25.
