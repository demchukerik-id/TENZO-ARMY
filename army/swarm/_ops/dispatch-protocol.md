# Swarm dispatch protocol

How Tenzo dispatches the swarm. Calibrated 2026-04-25 from Sato's first
dispatch (research/001).

---

## Default model per swarm role

| Role | Default model | When to override |
|---|---|---|
| **Sora · SCOUT** (internal research) | Haiku 4.5 | Sonnet for >100k-token reads or reconciling contradictory code paths |
| **Sato · SAGE** (external research) | Haiku 4.5 | Sonnet for reconciling contradictory web sources or producing structured output Tenzo will use unverified |
| **Goro · RUNNER** (execution) | Sonnet 4.6 | Opus when an irreversible operation requires careful judgment |
| **Mino · WATCHER** (monitoring) | Haiku 4.5 | Stays Haiku — pure I/O |
| **Fude · SCRIBE** (ledgers) | Haiku 4.5 | Sonnet if ledger updates need real synthesis (rare) |
| **Soji · CURATOR** (file org) | Haiku 4.5 | Sonnet when judging "is this superseded" requires reasoning over file contents |

**Why Haiku as default:** 5× cheaper than Opus, ~2× faster than Sonnet.
Subagent work is mostly I/O — fetch, read, summarize — where Opus
reasoning is wasted. Add prompt caching on shared system prompts for
~90% off cached input (~$0.10/MTok).

**Avoid Opus 4.6 Fast Mode** ($30/$150 per MTok) for any swarm dispatch
— latency win isn't worth 6× the bill.

---

## Default parallel count per dispatch type

| Dispatch type | Default count | Cap |
|---|---|---|
| Simple fact-finding | 1 | 1 |
| Direct comparison ("compare 4 models", "audit 5 endpoints") | **3** | 5 (= number of clearly independent dimensions) |
| Deep investigation ("trace this F5 phantom-stock bug") | 1 | 2–3 only when hypotheses are distinct + non-overlapping |
| Open-ended research with clear sub-questions | 3–5 | 5 without hierarchy; beyond 5 = hierarchical dispatch (Tenzo → sub-commander → workers) |

**Rules:**

1. **Never exceed 5 peers** without hierarchical splitting. Tenzo's
   synthesis cost dominates beyond that.
2. **Independent directions only.** If subagents would re-fetch the same
   files or contradict each other on the same question, collapse to 1.
3. **Deep investigation starts at 1.** Only fork on a real branching of
   hypotheses — don't pre-emptively parallelize.

---

## Standard brief format (Tenzo → swarm member)

Use this format when invoking a swarm member via the Agent tool. Keeps
calibration consistent across dispatches.

```
You are <NAME> — the <ROLE LABEL> subagent in the wasp-katana sync
project's swarm system. You report to Tenzo (Claude Opus 4.7).

GOAL
[one sentence — what success looks like]

QUESTIONS / TASKS
[numbered list, ≤3 per dispatch]

CONSTRAINTS
- Stay focused; do not chase tangents.
- Cite source URLs for any specific numerical claim.
- If the answer isn't accessible, say so — do not fabricate.
- ≤<word_cap> words in your final response.

OUTPUT FORMAT
[explicit structure with placeholders]

CONFIDENCE
State high/medium/low per question with one-line reasoning.
```

---

## Continuation across turns

When a swarm member returns useful state, capture their `agentId` from
the Agent tool's response and use `SendMessage({to: agentId})` to
continue them in subsequent turns. Otherwise they are re-spawned fresh.

Swarm members are terminated at session end (stateless across sessions
by design — calibration travels via this `_ops/` folder, not via agent
state).

---

## Audit trail

Each significant research dispatch gets a numbered file in
`_ops/research/NNN-YYYY-MM-DD-<topic>.md` with the brief + the
response + tokens/duration metadata. This is what makes future
calibration possible — Tenzo can read past dispatches to learn what
worked and what wasted budget.
