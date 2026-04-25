# TENZO-ARMY push procedure

How Tenzo pushes the current state of `fixes/context/` to the
TENZO-ARMY GitHub repo at https://github.com/demchukerik-id/TENZO-ARMY.

## Architecture

- **Source of truth:** `fixes/context/` inside the production project
  (`PRODUCTION_wasp-katana-sync/`).
- **Staging dir:** `C:/Users/demch/repos/TENZO-ARMY/` — a clone of the
  remote, outside OneDrive sync (OneDrive corrupts `.git` internals).
- **Remote:** `origin` = https://github.com/demchukerik-id/TENZO-ARMY.git
- **Default branch:** `main`.

The staging dir is the *only* place git operations run for this repo.
The production project's `.git` is for the wasp-katana code; we keep
TENZO-ARMY commits cleanly separated.

## First push (already done — for reference)

```bash
mkdir -p 'C:/Users/demch/repos'
cd 'C:/Users/demch/repos'
git clone https://github.com/demchukerik-id/TENZO-ARMY.git
cd TENZO-ARMY
cp -r 'C:/Users/demch/OneDrive/Desktop/PRODUCTION_wasp-katana-sync/fixes/context/.' .
git add .
git -c user.email='demchukerik@gmail.com' -c user.name='Erik' commit -m "<message>"
git branch -M main
git push -u origin main
```

Result: commit `ccd98a9` at 2026-04-25, 39 files / 3363 lines.

## Subsequent push (use this every time Erik says "push context")

```bash
cd 'C:/Users/demch/repos/TENZO-ARMY'

# Pull in case of any external changes (rare but possible)
git pull --rebase

# Mirror current source-of-truth into staging dir, deleting anything
# that was removed since last push (so trash moves get reflected,
# obsolete files don't linger)
rsync -av --delete \
  --exclude='.git' \
  'C:/Users/demch/OneDrive/Desktop/PRODUCTION_wasp-katana-sync/fixes/context/' \
  'C:/Users/demch/repos/TENZO-ARMY/'

# Commit + push
git add -A
git -c user.email='demchukerik@gmail.com' -c user.name='Erik' commit -m \
  "Session push — NNN-DAY-YYYY-MM-DD"
git push origin main
```

Branch naming for the commit message follows the convention from the
existing `KATANA-WASP-FINAL-DEBUG` repo (`NNN-DAY-YYYY-MM-DD`).

## What gets pushed

Everything in `fixes/context/` — verbatim. That includes:
- `army/` (all members + swarm)
- `_trash/` (Soji's per-session archives)
- `HANDOFF.md` / `STATUS.md` / `MAP.md` (onboarding)
- `2026-04-23-to-04-24-session.md` (historical session log)

What is **not** pushed:
- Anything outside `fixes/context/`
- Production code (wasp-katana repo handles that separately)
- `CLAUDE.md` at project root (lives with the production repo, not here)
- The shared-memory entries at `~/OneDrive/Documents/shared-memory/` (those are user-level)

## Failure modes to watch for

- **`git pull` conflicts** — rare, but if Erik ever pushes from a
  different machine or via the GitHub web UI, the staging dir may
  fall behind. `git pull --rebase` first; resolve in staging dir; push.
- **rsync deleting something it shouldn't** — the `--delete` flag is
  what makes Soji's trash moves and renames propagate. If anything
  important is ever lost, restore from `git log` history, not from
  the staging dir.
- **OneDrive-induced .git corruption** — only if someone accidentally
  moves the staging dir into OneDrive. Keep it at `C:/Users/demch/repos/`.

## Verification after push

```bash
cd 'C:/Users/demch/repos/TENZO-ARMY'
git log --oneline -3
git remote -v
```

Or visit https://github.com/demchukerik-id/TENZO-ARMY directly.
