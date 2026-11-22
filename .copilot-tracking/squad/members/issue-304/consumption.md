---
description: "Squad consumption ledger: members, models, estimated tokens, cost, and AI credits"
---

# Squad Consumption Ledger (Run: autopilot-run-45060332589)

## Attribution

| Role | Member | Agent | Model | Model Source | Priced As | Tier |
| ---- | ------ | ----- | ----- | ------------ | --------- | ---- |
| researcher |  | Squad Researcher | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| lead |  | Squad Lead | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| developer |  | Squad Implementor | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| tester |  | Squad Reviewer | Claude Haiku 5.5 (copilot) | host-reported | native credits | fast |
| orchestration |  | Coordinator+Scribe | claude-sonnet-5 | host-reported | native credits | mixed |

## Usage & Cost

| Role | Requests | In Tokens | Cached | Cache Wr | Out Tokens | Exact Cost (USD) | Exact Credits | Basis |
| ---- | -------- | --------- | ------ | -------- | ---------- | ---------------- | ------------- | ----- |
| researcher | 7 | 1,897 | 266,909 | 53,559 | 6,985 | 0.2342 | 23.42 | exact-native |
| lead | 4 | 2,140 | 132,341 | 24,162 | 3,791 | 0.1158 | 11.58 | exact-native |
| developer | 15 | 2,424 | 727,539 | 38,771 | 7,089 | 0.2454 | 24.54 | exact-native |
| tester | 12 | 1,717 | 616,158 | 76,093 | 13,908 | 0.0228 | 2.28 | exact-native |
| orchestration | 106 | 26,296 | 15,374,119 | 207,104 | 60,948 | 4.2547 | 425.47 | exact-native |
| **Total** | **144** | **34,474** | **17,117,066** | **399,689** | **92,721** | **4.8729** | **487.29** | **exact-native** |

### Derivation

```text
autopilot-run-45060332589.md — 0 block(s) — identities: (none)
Squad Implementor.md — 2 block(s) — identities: da827100,ca1a7e50
Squad Lead.md — 1 block(s) — identities: e46a8a15
Squad Researcher.md — 1 block(s) — identities: 2df5f4e7
Squad Reviewer.md — 2 block(s) — identities: b4ae7fb9,29e4e0cd
Squad Scribe.md — 10 block(s) — identities: 31307ad9,d5e0a496,58362d06,c74fa0a7,40cfdf86,9310d50e,b2b48fb0,6ac66952,1ef7af64,15dcbc2e
Native usage snapshot — session: 221b7e64-6853-42df-851d-3b457d91f0c6; records: 144; through: 2026-11-22T19:44:09.522Z; digest: D0C9C5C207906C312E43C7523A245C4708FEB1B11365AC9FEAB908CD9B62B9A7
researcher         requests 7      input 1897       cached 266909     cache-write 53559      output 6985       nano-AIU 23423240000 = 23.42 credits / 0.2342 USD
lead               requests 4      input 2140       cached 132341     cache-write 24162      output 3791       nano-AIU 11582910000 = 11.58 credits / 0.1158 USD
developer          requests 15     input 2424       cached 727539     cache-write 38771      output 7089       nano-AIU 24541940000 = 24.54 credits / 0.2454 USD
tester             requests 12     input 1717       cached 616158     cache-write 76093      output 13908      nano-AIU 2279890500 = 2.28 credits / 0.0228 USD
orchestration      requests 106    input 26296      cached 15374119   cache-write 207104     output 60948      nano-AIU 425465580000 = 425.47 credits / 4.2547 USD
total nano-AIU 487293560500 = 487.29 credits / 4.8729 USD
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
| Deliverables produced | 3 | derived |
| Accepted by an independent review | 0 | derived |
| Rejected | 0 | derived |
| Unreviewed | 3 | derived |
| Dispatches (excluding Scribe) | 6 | derived |
| Rework dispatches | 2 | derived |
| First-pass yield | n/a | derived |
| Exact credits per accepted deliverable | n/a | measured |
| Rework share of exact credits | n/a | unavailable without per-history-run identity |
| Orchestration share of exact credits | 87.3% | measured |

Value is counted only for deliverables an independent review-class verdict accepted; a produced but unaccepted deliverable adds cost and no value. A review that names deliverables covers only those it names. Rework counts a repeat of the same agent and deliverable path only: a fix by a different agent, to a different file, or with no deliverable path is not counted, so the rework figures are a floor. Compare runs only for like work, using medians of several runs. Source: FinOps Foundation unit-economics KPIs (cost per unit of value, error and retry waste).

## Cost Comparison (exact native actual)

| Scenario | Basis | AI Credits | USD |
| -------- | ----- | ----------: | --: |
| With HVE Squad through 2026-11-22T19:44:09.522Z | complete deduplicated native records; coordinator and descendants included | 487.29 | 4.8729 |
| Without HVE Squad: same exact role-request token categories on `Claude Sonnet 5.5 (copilot)`, no coordinator or Scribe | calculated counterfactual from exact request tokens | 98.98 | 0.9898 |

HVE Squad is **3.8831 USD (392.3%) higher** than the same exact role traffic on `Claude Sonnet 5.5 (copilot)`.

> The HVE Squad row is exact as of the native session snapshot: 144 complete accounting records, cumulative digest `D0C9C5C207906C312E43C7523A245C4708FEB1B11365AC9FEAB908CD9B62B9A7`. The without-Squad row is not an observed alternate run; it holds every exact role request's uncached input, cache read, cache write, and output counts constant, reprices them on one model, and excludes coordinator/Scribe overhead. It therefore measures routing savings on identical traffic and does not invent context growth, rework, or behavioral differences for a hypothetical manual run.
