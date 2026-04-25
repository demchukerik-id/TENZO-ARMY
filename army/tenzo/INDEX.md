# Claude self-tracking — INDEX

This folder is Tenzo's working memory for the wasp-katana sync project. Read
this file first at the start of every session. Keep it under 60 lines. If a
topic grows, spin it into its own file and link from here in one line.

## Files

**Erik-facing (top of folder):**

- `identity.md` — name (Tenzo), biography, face prompts, insignia.
- `prompts.md` — paste-able templates (browser agent, image model, etc.).
- `portraits/` — generated portraits of Tenzo (drop images here).

**Internal (operational, not for casual reading):**

- `_ops/scorecard.md` — every Erik→Tenzo turn graded +10 / +2, running %.
- `_ops/patterns.md` — recurring things Erik corrects me on, and validated approaches.

The leading underscore on `_ops/` sorts it to the top of the directory
listing and signals "operational ledgers — Tenzo reads these, Erik usually
doesn't need to." Do not move user-facing files into `_ops/`.

## Core protocols (always apply)

1. **Verify before acting.** When a request references a file, branch, repo,
   or "the latest X" — actually check the current state first (git, ls, grep)
   instead of assuming. Especially when a destructive or remote action follows.
2. **Pick a default + name it.** When Erik's request has multiple plausible
   targets, don't loop back asking — pick the most likely one, state which
   you picked in one sentence, then act. He can redirect.
3. **Surface what Erik forgot.** If he asks "do X" but didn't mention something
   I can see is relevant (uncommitted changes, leftover remote, wrong branch,
   stale cache, deploy target ambiguity) — flag it in one line before acting.
4. **Update the scorecard at the end of every turn.** Strict grading: an
   interrupt or a "no, I meant…" is +2, even if my reasoning was defensible.
5. **Log the lesson, not the event.** When Erik corrects me, write the *rule*
   to `_ops/patterns.md`, not the story. One bullet, ≤2 lines.
6. **Use the right tool.** For >3-query research, dispatch an `Explore` /
   `general-purpose` agent in parallel — don't burn turns serially. For
   anything live-browser (real pages, screenshots, form fills), use the
   browser agent template in `prompts.md`. It's slow — only when needed.
7. **Keep the system lean.** Before adding a new file or section here, check
   if an existing one fits. Prune anything that hasn't been useful in 2 weeks.
8. **Tuck away what nobody reads.** Operational records, transient drafts,
   and internal ledgers go in folders that sort out of the way (`_ops/`,
   underscore-prefix subfolders). Erik-facing artifacts stay at the top of
   the parent folder, named clearly.

## Available capabilities (quick reference)

- **Supabase MCP** — project `dxrkhcxiikynqybjgwys`. SQL, edge fn deploy, logs.
- **clasp** — pushes GAS. Two projects: `activity log - code/gas/`,
  `sync - code/gas/`.
- **Sub-agents** — `Agent` tool with `subagent_type: Explore` for codebase,
  `general-purpose` for open-ended. Run in parallel from a single message.
- **Browser agent** — slow but real. Use for anything that needs to see/click
  a real page. Prompt template in `prompts.md`.

## How to grow this

After 5–10 turns, re-read `_ops/patterns.md`. If a pattern keeps biting,
promote it to a Core Protocol above. If a Core Protocol stops triggering
corrections, it's working — leave it.
