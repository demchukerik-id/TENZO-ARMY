# Research dispatch 001 — Model selection + parallel-count defaults

**Dispatched by:** Tenzo
**Performed by:** Sato (SAGE — external research subagent)
**Date:** 2026-04-25
**Tokens used:** ~47,820
**Tool uses:** 11
**Duration:** ~75 seconds

---

## Question 1 — Best Claude model for search/exploration subagent work

| Model | Input $/MTok | Output $/MTok | Speed (rough) | Notes |
|---|---|---|---|---|
| Opus 4.7 | $5 | $25 | slow (Opus class) | New tokenizer can produce ~35% more tokens for same text |
| Opus 4.6 | $5 | $25 | slow | Opus 4.6 also has Fast Mode at 6× ($30/$150) |
| Sonnet 4.6 | $3 | $15 | ~40–60 tok/s | 1M-token context at base price |
| Haiku 4.5 | $1 | $5 | ~80–120 tok/s (~2× Sonnet) | Cheapest + fastest in this set |

**Recommendation (cost > speed > quality):** Default research subagent =
**Haiku 4.5**. It's 5× cheaper than Opus on both input and output, ~2×
faster than Sonnet, and Anthropic's own multi-agent reference architecture
uses a stronger lead + cheaper subagents (Opus lead, Sonnet/Haiku workers)
precisely because subagent work is mostly I/O-bound fetching + summarization,
where raw reasoning headroom is wasted. Add prompt caching on shared
system prompts for another ~90% off cached input ($0.10/MTok).

**Override to Sonnet 4.6** when the subagent must (a) reconcile contradictory
sources requiring real reasoning, (b) read >100k tokens of context, or (c)
produce structured output the parent will trust without verification.

**Override to Opus 4.7** only when correctness on a single answer matters
more than 5× the cost — rare for a search subagent.

**Avoid Opus 4.6 Fast Mode** ($30/$150) for research; the latency win isn't
worth 6× the bill.

## Question 2 — Optimal parallel research count

Anthropic's published guidance (multi-agent research engineering post):
- Simple fact-finding → 1 agent
- Direct comparisons → 2–4 subagents
- Complex/open-ended research → 10+ with clearly divided responsibilities
- Failure mode: agents spawning 50 subagents for trivial queries

Their lead-agent default spawns 3–5 in parallel, and they report 90.2%
improvement vs. single-agent Opus on research evals — but only when work
is genuinely parallelizable (independent directions, breadth-first).

**Default for wasp-katana = 3.** Rule for exceeding:
- **Broad-sweep / comparison** ("compare 4 models", "audit 5 endpoints"):
  scale to N where N = number of clearly independent dimensions, capped at 5.
- **Deep-investigation** ("trace this F5 phantom-stock bug"): start with **1**.
  Only fork to 2–3 when you have distinct, non-overlapping hypotheses to
  test in parallel — otherwise subagents step on each other and re-fetch
  the same files.
- **Never exceed 5** without splitting into hierarchical dispatch (Tenzo →
  sub-commander → workers). Beyond ~5 peers, Tenzo's synthesis cost
  dominates.

## Sources

- https://platform.claude.com/docs/en/about-claude/pricing
- https://www.anthropic.com/engineering/multi-agent-research-system
- https://www.anthropic.com/news/claude-haiku-4-5
- https://artificialanalysis.ai/models/comparisons/claude-sonnet-4-6-vs-claude-4-5-haiku

## Confidence

- Q1: **high** — pricing pulled directly from Anthropic's official docs
- Q2: **high** for the 3-default and 2–4 / 10+ tiers (Anthropic's own
  published guidance); **medium** on the wasp-katana-specific cap of 5,
  which is Sato's extrapolation, not a published number

---

## Sato's continuation handle

Sato remained spawned with agentId `a620f2d5c03164dab` — Tenzo can
SendMessage to him for follow-ups in this session. Will be terminated
at session end.
