# Research dispatch 002 — po-resync discovery + sheet color root cause

**Dispatched by:** Tenzo
**Performed by:** 3× Sora subagents (Explore type) in parallel
**Date:** 2026-04-25
**Goal:** Map ST-resync's design + PO's current state + sheet color logic so Tenzo can plan the po-resync feature.

---

## ST-resync — the model to mirror

**Lives at:** `activity log - code/supabase/functions/st-resync/index.ts` (367 lines, single file).

**Trigger surface (single — manual only):**
- `activity log - code/gas/01_Logging.js:909` — `onEdit()` fires when user picks "Resync" from the per-line Failed/Skipped dropdown.
- Calls `stResyncViaApi_(lineId)` at `01_Logging.js:995` which POSTs `{ line_id: N }` to st-resync edge function.
- No cron, no auto-retry. Design is intentional: operator clicks Resync after fixing whatever caused the failure (e.g., adjusting WASP stock).

**Step-by-step:**
1. Receive POST with `line_id: N`.
2. Load line from `st_sync_lines`; load parent `st_sync_records`.
3. Validate: bail if record status is `reverted` (409); reject zero-qty or `wasp_skip` (400).
4. Pre-check WASP stock at remove location.
5. Lot resolution (3-tier fallback): Katana snapshot's `batch_transactions` → `waspResolveSmallestLot()` (relaxed=true) → null.
6. Transactional WASP add-then-remove with rollback:
   - Write `echo_guard` to suppress inbound webhook reflection.
   - POST `/public-api/transactions/item/add` at destination.
   - POST `/public-api/transactions/item/remove` at source (only if add succeeded).
   - If remove fails after add succeeded: immediately reverse add via another remove call (rollback).
7. DB updates: `st_sync_lines` (success, qty_applied, error, src_lot, dst_lot, etc); `st_sync_records.status` recomputed (done | partial | reverted).
8. Notify GAS via `GAS_WEBHOOK_URL` POST — full `st_activity_log` payload — GAS re-renders the block via `upsertActivityByRefId_()`.

**State changes:**
- DB: `st_sync_lines` (line 324–331), `st_sync_records` (line 337–341).
- Katana: NONE (read-only via stored snapshot).
- WASP: `add` + `remove` endpoints (line 294, 301), with rollback at line 305.
- Sheet UI: cell color "Retrying..." blue while in flight, then GAS webhook re-renders to green/yellow/red.

**5 patterns Tenzo should copy for po-resync:**
1. Transactional pair (add → remove with inline rollback) + `echo_guard` for both calls.
2. Lot resolution fallback chain (snapshot → smallest-lot resolver with relaxed=true → null).
3. Defensive WASP response parsing (`HasError`, `ErrorCount`, `SuccessfullResults`, ResultCode, "record N is fail" pattern).
4. Line validation before execution (reject zero-qty, missing direction, wasp_skip; pre-check source stock).
5. GAS webhook re-render with full payload + `upsertActivityByRefId_()` for in-place update.

---

## PO today — the gap to fill

**po-sync** (`activity log - code/supabase/functions/po-sync/index.ts`):
- Per-row delta reconciliation against Katana PO receives.
- Main loop (line 583): acquires `po_sync_lock`, fetches `po_check_queue` (max 20/tick), processes each via `processOne` (line 601).
- Exit conditions: 55s time limit (TIME_LIMIT_MS, line 598) or queue exhausted; self-kicks if work remains (line 631).
- Auto-rescue: `po_sync_records.status='recorded'` >2 min triggers po-adjust re-kick (line 612–624).

**po-reconcile** (`activity log - code/supabase/functions/po-reconcile/index.ts`):
- Enqueuer for RECEIVED / PARTIALLY_RECEIVED POs.
- Always scans PARTIALLY_RECEIVED without date filter (line 146) — Katana never updates `updated_at` on 2nd+ partial receives.

**po-adjust** (`activity log - code/supabase/functions/po-adjust/index.ts`):
- WASP mutator. Executes `add` and `revert_add` lines transactionally with rollback.
- Called from po-sync's auto-rescue (line 618) and from GAS webhook.

**Current retry / recovery (4 mechanisms):**
1. Lot-pending guard (line 530): defer all lines if `src_lot=null` and Katana has `batch_transactions`. Re-enqueued by po-reconcile next scan.
2. Stuck-after-partial guard v9 (line 535–568): skips byte-identical retry, marks inbox `stuck_after_partial: operator Resync required`.
3. Hysteresis v8 (line 466, 484, 497): `COMPARE_TOLERANCE = 0.5` units to avoid false-positive cycles on Katana's sub-unit float noise.
4. Auto-rescue loop (line 612–624): re-kicks po-adjust on records stuck >2 min in `recorded`.

**THE GAP:** No `po-resync` edge function exists. No PO Resync dropdown in the activity sheet. v9's "operator Resync required" message currently has nothing to back it.

**What needs to change to add po-resync:**
- New `po-resync/index.ts` mirroring st-resync (line-id-based POST, transactional add-remove with rollback, GAS webhook re-render).
- DB: `po_sync_lines` should have schema parity with `st_sync_lines` (src_success, src_lot, src_expiry, dst_*, qty_applied, error). **Verify before coding.**
- Sheet UI: extend `01_Logging.js` PO section (~line 332) with Failed/Skipped → Resync dropdown matching ST pattern (line 347 `requireValueInList([stLabel, 'Resync'])`). Hook `onEdit` to POST to po-resync.
- Cutover concern: should v9 stuck-after-partial guard soften once po-resync exists? Currently it blocks all retries; after po-resync, that's redundant for the recoverable cases.

---

## Sheet color — root cause + one-line fix

**File:** `activity log - code/gas/01_Logging.js`
**Lines:** 548–553 in `renderActivityBlockAtRow_`

**Current code:**
```javascript
var bgGrid = [];
for (var si = 0; si < subBgColors.length; si++) {
  var colors = subBgColors[si];
  bgGrid.push([null, null, null, colors[3] || null, colors[4] || null, colors[5] || null]);
}
range.setBackgrounds(bgGrid);
```

Columns 1–3 (SKU, blank, blank) are explicitly `null` → render white. Columns 4–6 get the detail row's color.

**Recommended change (line 551):**
```javascript
bgGrid.push([colors[3] || null, colors[3] || null, colors[3] || null, colors[3] || null, colors[4] || null, colors[5] || null]);
```

Apply detail row color to all 6 columns uniformly.

**Hex:** `#e8f5e9` (light green) — matches existing successful-status palette. If Erik prefers more vivid: `#B7E1CD` (muted green) or `#00FF00` (vivid).

**Cross-flow cascade:** This is a SHARED code path — change cascades to ALL flows (PO/MO/F5/SA/SO/ST). No per-flow updates needed; one line, one effect everywhere. If Erik wants ST-only behavior, would need an `if (flow === 'ST')` branch.

---

## Confidence

- ST-resync research: **HIGH** — full code read, patterns explicit, single trigger surface confirmed.
- PO research: **HIGH** — code + recent fix docs + v8/v9 deployment notes all consistent.
- Sheet color research: **HIGH** — root cause unambiguous, fix is one line, cascade is mechanical.

---

## Sora dispatches summary

| # | Subagent | Topic | Tokens | Tool uses | Outcome |
|---|---|---|---|---|---|
| 2a | Sora-1 (Explore) | st-resync feature deep-dive | (incl. in turn) | (incl. in turn) | High-confidence design map, 5 patterns to copy |
| 2b | Sora-2 (Explore) | PO retry mechanism + gap | (incl. in turn) | (incl. in turn) | High-confidence current state + change list |
| 2c | Sora-3 (Explore) | Sheet color white→green | (incl. in turn) | (incl. in turn) | Root cause found, one-line fix, cross-flow cascade confirmed |

3 agents in parallel, single turn delivery, all high confidence. Calibration on dispatch-protocol.md (parallel default of 3 for broad-sweep) — confirmed on this run.
