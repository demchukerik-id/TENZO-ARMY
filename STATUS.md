# Production status — wasp-katana sync

Snapshot of what's deployed, what's open, what's under observation.
Updated at every session push.

Last updated: 2026-04-25 (rolled forward from 2026-04-23/24 session doc
+ inflight observations).

---

## Deployed and stable

| Flow | Version | What was fixed |
|---|---|---|
| **F5 sync** | v25 | drain-speed fixes — `CIRCUIT_BREAKER_THRESHOLD` 2→5, `markQueuePoll` resets `rate_limit_hits`, location-fetch 429 reclassified as `location_fetch_transient`, `INTER_ITEM_DELAY_MS` 300→150, `BACKOFF_LADDER_MS[0]` 30s→15s |
| **MO sync** | (SQL + edge fn) | unique-lock — `acquire_mo_poll_lock` generates unique caller ID via `gen_random_uuid()`; partial unique index `mo_sync_records_one_active`; 23505 dup-handling in `saveRecord`; defer-empty guard for empty Katana movements arrays |
| **PO sync** | v8 | hysteresis — `COMPARE_TOLERANCE` 0.0001→0.5 at all 3 delta-comparison sites in `processOne` |
| **PO sync** | v9 / v11 in Supabase | stuck-after-partial guard — skips byte-identical retry, requires operator Resync |
| **SA echoguard** | (deployed) | location-mismatch fix |

---

## Open / under observation

| Flow | Issue | Doc | State |
|---|---|---|---|
| **F5** | Throughput ceiling — Katana QPS is the binding constraint after v23/v25 | `fixes/active/F5/2026-04-24-throughput-ceiling.md` | 4 fix candidates ranked (variant cache / location cache / global token bucket / cron stagger). No decision. |
| **MO** | Lot-tagged stock excluded when recipe lot=null (`mo-adjust:126`) | `fixes/active/MO/2026-04-24-mo-adjust-no-lot-skips-lot-tagged-stock.md` | One-line fix ready, **NOT YET DEPLOYED**. Same pattern likely in `f5-adjust`, `st-adjust`, `po-adjust` — flagged for audit. |
| **PO** | Reconcile hysteresis | `fixes/active/PO/2026-04-24-po-reconcile-hysteresis.md` | Deployed v8, in 24h observation window. PO-648 stopped cycling immediately. |
| **PO** | Stuck-after-partial guard | `fixes/active/PO/2026-04-24-po-stuck-after-partial.md` | Deployed v9, in 24h observation window. Zero new PO-726 sync_records since deploy. |
| **SA** | Multi-lot SA collapse → silent inventory drift | `fixes/active/SA/2026-04-24-multi-lot-sa-collapses-to-one-lot.md` | Documented (SA-2367299: 14 EGG-X drift across 4 lots). 4 fix candidates. WASP webhook template fix is highest impact. |
| **SA** | WASP/Katana item-config drift trap (WASP-TEST-001) | `fixes/active/SA/2026-04-24-wasp-katana-item-config-drift.md` | 4 mitigations ranked. M1 (pre-sync validation gate) most impactful. No fix shipped. |
| **ST** | No per-line retry + 9+ days webhook silence | `fixes/ST/2026-04-23-st-no-retry-and-webhook-silence.md` | Reconcile cron keeping ST alive; webhook silence root cause unclear. |
| **sync-sheet** | WASP-only state after location move | `fixes/sync-sheet/2026-04-24-wasp-only-after-location-move.md` | Documented. |
| **new-features** | Cross-flow Katana rate-limit handling | `fixes/new-features/2026-04-24-katana-rate-limit-handling.md` | Cross-cutting. 4 fixes proposed (cron stagger / per-flow retry+backoff / shared token-bucket / tier upgrade). Recommendation: ship Fix 2 first. |

---

## WASP support items (waiting on WASP)

- Add-to-existing-lot doesn't auto-populate expiry date — `fixes/wasp-questions/2026-04-24-add-to-existing-lot-no-expiry-autofill.md`

---

## Connections / tools

- **Supabase MCP** — project ref `dxrkhcxiikynqybjgwys`, URL `https://dxrkhcxiikynqybjgwys.supabase.co`. Connected.
- **clasp** — logged in (`~/.clasprc.json`). Two GAS projects bound:
  - activity log — script `1sV_rXLdEsKSFMrwQLTGTihwwO-pHDR87zrRs6vhYGaUcRbd9kIm-0ulH`, path `activity log - code/gas/`
  - sync — script `1AjccwZFrbBmKfAbRKNbAiFrESWfJzBApF_CwAvkgNQ0MmB54lT83DUjc`, path `sync - code/gas/`
- **GitHub remote** — `KATANA-WASP-FINAL-DEBUG` repo. Existing branches: `001-WED-2026-04-22`, `002-THU-2026-04-23`, `003_FRI_2026-04-24` (current), `004_FRI_2026-04-24`, `main`.

---

## Strategic / cross-cutting

- **Katana rate-limit handling.** Multiple flows hit 429s; affects perceived speed. Decision pending on cross-cutting approach.
- **`fixes/` folder reorganization.** Partial. Active issues moved to `fixes/active/`, but `fixes/solved/` reorganization is incomplete — legacy folders `fixes/F5/`, `fixes/MO/`, `fixes/PO/`, `fixes/SA/`, `fixes/ST/` still hold solved docs. The remote `004_FRI_2026-04-24` branch has the full reorganization with 22 renames pending; not yet pulled into local.
- **Resync universalization.** Operator-facing per-line Resync planned across all flows; once universal, the "adjust from Katana tab in Sync sheet" workaround gets removed (committed direction from Erik).

---

## Next session — likely first asks

Based on `HANDOFF.md` open decisions:
- Resolve which GitHub repo + push trigger phrase
- Decide Taka regeneration path (label-swap vs full regen)
- Possibly deploy MO line-126 fix (one-liner, ready)
