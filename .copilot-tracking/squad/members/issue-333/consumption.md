---
description: "Squad consumption ledger: members, models, estimated tokens, cost, and AI credits"
---

# Squad Consumption Ledger (Run: autopilot-issue-333-1)

## Attribution

| Role | Member | Agent | Model | Model Source | Priced As | Tier |
| ---- | ------ | ----- | ----- | ------------ | --------- | ---- |
| researcher |  | Squad Researcher | gpt-6.2-sol | cli-pinned | GPT-6.2 Sol | default |
| lead |  | Squad Lead | gemini-4-argon | cli-pinned | Gemini 4 Argon | default |
| developer |  | Squad Implementor | gemini-4-argon | cli-pinned | Gemini 4 Argon | default |
| tester |  | Squad Reviewer | gemini-4-argon | cli-pinned | Gemini 4 Argon | fast |
| architect |  | System Architecture Reviewer | gemini-4-carbon | cli-pinned | Gemini 4 Carbon | default |
| cost-manager |  | Squad Cost Manager | gemini-4-carbon | cli-pinned | Gemini 4 Carbon | default |
| security |  | Security Planner | gemini-4-carbon | cli-pinned | Gemini 4 Carbon | default |
| technical-writer |  | Squad Technical Writer | gemini-4-argon | cli-pinned | Gemini 4 Argon | fast |
| prompt-engineer |  | Squad Prompt Engineer | gemini-4-argon | cli-pinned | Gemini 4 Argon | default |
| challenger |  | Squad Challenger | gemini-4-argon | cli-pinned | Gemini 4 Argon | default |
| orchestration |  | Squad Scribe | claude-sonnet-5.5 | session-inherited | Claude Sonnet 5.5 | mixed |

## Usage & Cost

| Role | Turns | In Tokens | Cached | Cache Wr | Out Tokens | Est. Cost (USD) | Est. Credits | Basis |
| ---- | ----- | --------- | ------ | -------- | ---------- | ---------------- | ------------ | ----- |
| researcher | 12 | 148,800 | 595,200 | 84,000 | 15,000 | 1.3592 | 135.92 | estimated |
| lead | 15 | 264,000 | 1,056,000 | 0 | 30,000 | 0.9336 | 93.36 | estimated |
| developer | 105 | 3,402,000 | 13,608,000 | 0 | 210,000 | 10.2648 | 1,026.48 | estimated |
| tester | 18 | 302,400 | 1,209,600 | 0 | 27,000 | 0.9958 | 99.58 | estimated |
| architect | 10 | 136,000 | 544,000 | 0 | 15,000 | 0.4764 | 47.64 | estimated |
| cost-manager | 10 | 136,000 | 544,000 | 0 | 15,000 | 0.4764 | 47.64 | estimated |
| security | 10 | 136,000 | 544,000 | 0 | 15,000 | 0.4764 | 47.64 | estimated |
| technical-writer | 35 | 1,134,000 | 4,536,000 | 0 | 70,000 | 3.4216 | 342.16 | estimated |
| prompt-engineer | 70 | 2,268,000 | 9,072,000 | 0 | 140,000 | 6.8432 | 684.32 | estimated |
| challenger | 18 | 302,400 | 1,209,600 | 0 | 27,000 | 0.9958 | 99.58 | estimated |
| orchestration | 56 | 218,400 | 873,600 | 336,000 | 44,800 | 1.8122 | 181.22 | estimated |
| **Total** | **359** | **8,448,000** | **33,792,000** | **420,000** | **608,800** | **28.0553** | **2,805.53** | |

### Derivation

```text
Security Planner.md — 1 block(s) — identities: 796138f0
Squad Challenger.md — 1 block(s) — identities: 18db68a3
Squad Cost Manager.md — 1 block(s) — identities: 5df7d578
Squad Implementor.md — 3 block(s) — identities: 0f2f77d6,0fb0841d,b79bc6bb
Squad Lead.md — 1 block(s) — identities: 4feffd33
Squad Prompt Engineer.md — 2 block(s) — identities: d149ad1e,0c94fbb4
Squad Researcher.md — 1 block(s) — identities: 61812889
Squad Reviewer.md — 1 block(s) — identities: cc9a283b
Squad Scribe.md — 14 block(s) — identities: 1f36591e,f2a24646,1587a1b0,e52c8cbc,062161dd,e6e671b4,82af9f41,3b377f29,76c7707d,eadbabf0,6555a35f,eee23abb,fdf18971,f6220f06
Squad Technical Writer.md — 1 block(s) — identities: 6dabfe95
System Architecture Reviewer.md — 1 block(s) — identities: 1a63c744
researcher     turns 12           148800 × 4.00 + 595200 × 0.20 + 84000 × 5.00 + 15000 × 15.00 = 1359240 / 1e6 = 1.3592
lead           turns 15           264000 × 2.00 + 1056000 × 0.10 + 0 × 0.00 + 30000 × 10.00 = 933600 / 1e6 = 0.9336
developer      turns 35+35+35=105 3402000 × 2.00 + 13608000 × 0.10 + 0 × 0.00 + 210000 × 10.00 = 10264800 / 1e6 = 10.2648
tester         turns 18           302400 × 2.00 + 1209600 × 0.10 + 0 × 0.00 + 27000 × 10.00 = 995760 / 1e6 = 0.9958
architect      turns 10           136000 × 2.00 + 544000 × 0.10 + 0 × 0.00 + 15000 × 10.00 = 476400 / 1e6 = 0.4764
cost-manager   turns 10           136000 × 2.00 + 544000 × 0.10 + 0 × 0.00 + 15000 × 10.00 = 476400 / 1e6 = 0.4764
security       turns 10           136000 × 2.00 + 544000 × 0.10 + 0 × 0.00 + 15000 × 10.00 = 476400 / 1e6 = 0.4764
technical-writer turns 35           1134000 × 2.00 + 4536000 × 0.10 + 0 × 0.00 + 70000 × 10.00 = 3421600 / 1e6 = 3.4216
prompt-engineer turns 35+35=70     2268000 × 2.00 + 9072000 × 0.10 + 0 × 0.00 + 140000 × 10.00 = 6843200 / 1e6 = 6.8432
challenger     turns 18           302400 × 2.00 + 1209600 × 0.10 + 0 × 0.00 + 27000 × 10.00 = 995760 / 1e6 = 0.9958
orchestration  turns 4+4+4+4+4+4+4+4+4+4+4+4+4+4=56 218400 × 2.00 + 873600 × 0.10 + 336000 × 2.50 + 44800 × 10.00 = 1812160 / 1e6 = 1.8122
                                                                                        total = 28.0553
```

> This is the initialization and fallback shape. Exact mode replaces `Turns` with
> `Requests`, `Est. Cost` / `Est. Credits` with `Exact Cost` / `Exact Credits`, and
> every row's basis with `exact-native`. Its input column is uncached billed input;
> cache read and cache write remain separate, so the four token columns are
> disjoint and reproduce native token-detail charges. The exact Attribution table
> uses `host-reported` and `native credits`; fallback Attribution preserves the
> existing model-source ladder. 1 AI credit = 0.01 USD.
>
> The `<file>.md — <n> block(s) — identities: <hash>,<hash>,...` line uses the history file's basename, without a `history/` prefix. It is the ledger's own record of which `###` entries it has already folded in, one short deterministic hash per entry in file order. The initial Scribe-only seed records an empty prefix; the first ledger write replaces it with actual identities. A rewrite that finds this run's recorded identities are not an ordered prefix of the file's current identities — same count but different hashes, or fewer current entries than recorded — means an entry was overwritten, reordered, or removed since the last rewrite rather than only appended to, and `Measure-SquadLedger.ps1 -Check` (or render mode) refuses rather than silently accepting it. A pre-existing ledger with no recorded identities at all (an older-format entry) only warns when checked plainly; it never fails on that account alone. But the Scribe's own post-write self-check always runs `-Check` together with `-ExpectedHistoryCounts` (*scribe-procedure.md*'s Write-Completeness Self-Check Step 3), and in that combination a Derivation missing identities entirely, or missing them for only some of the touched files (a partial paste), FAILS instead of warning — that call always follows a fresh write, so a missing or partial paste there is this run's own defect, never a genuinely old ledger.

## Unit Economics (value)

| Measure | Value | Label |
| ------- | ----- | ----- |
| Deliverables produced | 11 | derived |
| Accepted by an independent review | 0 | derived |
| Rejected | 0 | derived |
| Unreviewed | 11 | derived |
| Dispatches (excluding Scribe) | 13 | derived |
| Rework dispatches | 0 | derived |
| First-pass yield | n/a | derived |
| Est. credits per accepted deliverable | n/a | estimated |
| Rework share of est. credits | 0.0% | estimated |
| Orchestration share of est. credits | 6.5% | estimated |

Value is counted only for deliverables an independent review-class verdict accepted; a produced but unaccepted deliverable adds cost and no value. A review that names deliverables covers only those it names. Rework counts a repeat of the same agent and deliverable path only: a fix by a different agent, to a different file, or with no deliverable path is not counted, so the rework figures are a floor. Compare runs only for like work, using medians of several runs. Source: FinOps Foundation unit-economics KPIs (cost per unit of value, error and retry waste).

## Cost Comparison (illustrative)

This run consumed an estimated **$28.0553 (~2805.53 AI credits)** across 10 specialized agent(s) (squad figure only; native per-role attribution unavailable; manual baseline and saving unavailable).
