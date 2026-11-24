---
description: "Squad consumption ledger: members, models, estimated tokens, cost, and AI credits"
---

# Squad Consumption Ledger (Run: autopilot-run-45243130867)

## Attribution

| Role | Member | Agent | Model | Model Source | Priced As | Tier |
| ---- | ------ | ----- | ----- | ------------ | --------- | ---- |
| researcher |  | Squad Researcher | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| lead |  | Squad Lead | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| developer |  | Squad Implementor | Claude Sonnet 5.5 (copilot) | host-reported | native credits | default |
| tester |  | Squad Reviewer | Claude Haiku 5.5 (copilot) | host-reported | native credits | fast |
| orchestration |  | Coordinator+Scribe | Claude Haiku 5.5 (copilot), claude-sonnet-5 | host-reported | native credits | mixed |

## Usage & Cost

| Role | Requests | In Tokens | Cached | Cache Wr | Out Tokens | Exact Cost (USD) | Exact Credits | Basis |
| ---- | -------- | --------- | ------ | -------- | ---------- | ---------------- | ------------- | ----- |
| researcher | 10 | 2,922 | 401,687 | 61,111 | 14,722 | 0.3460 | 34.60 | exact-native |
| lead | 8 | 2,914 | 319,356 | 40,211 | 9,926 | 0.2376 | 23.76 | exact-native |
| developer | 18 | 12,516 | 1,020,443 | 61,464 | 12,921 | 0.4099 | 40.99 | exact-native |
| tester | 11 | 2,079 | 518,539 | 66,363 | 15,378 | 0.0214 | 2.14 | exact-native |
| orchestration | 497 | 42,168 | 123,536,645 | 509,903 | 328,620 | 29.1246 | 2912.46 | exact-native |
| **Total** | **544** | **62,599** | **125,796,670** | **739,052** | **381,567** | **30.1395** | **3013.95** | **exact-native** |

### Derivation

```text
autopilot-run-45243130867.md — 0 block(s) — identities: (none)
Squad Implementor.md — 1 block(s) — identities: 06c20bbf
Squad Lead.md — 1 block(s) — identities: 3ffd4fd6
Squad Researcher.md — 1 block(s) — identities: 7e7ebc93
Squad Reviewer.md — 1 block(s) — identities: ba31a78d
Squad Scribe.md — 8 block(s) — identities: e9e510fc,af9fc2d0,f45d1459,fe89e6f2,1db65864,ca5224fc,0581ded6,0b7f817a
Native usage snapshot — session: aa8fee56-88f5-4bc6-a1d7-e072e9ac37cd; records: 544; through: 2026-11-23T20:07:09.169Z; digest: F3969F433ADCCAFA88AAD93BEA9E7F8337CB0AD23CE7772CE1C9C24287ABF7ED
researcher         requests 10     input 2922       cached 401687     cache-write 61111      output 14722      nano-AIU 34601020000 = 34.60 credits / 0.3460 USD
lead               requests 8      input 2914       cached 319356     cache-write 40211      output 9926       nano-AIU 23755110000 = 23.76 credits / 0.2376 USD
developer          requests 18     input 12516      cached 1020443    cache-write 61464      output 12921      nano-AIU 40994630000 = 40.99 credits / 0.4099 USD
tester             requests 11     input 2079       cached 518539     cache-write 66363      output 15378      nano-AIU 2137766500 = 2.14 credits / 0.0214 USD
orchestration      requests 497    input 42168      cached 123536645  cache-write 509903     output 328620     nano-AIU 2912459542500 = 2912.46 credits / 29.1246 USD
total nano-AIU 3013948069000 = 3013.95 credits / 30.1395 USD
NOTE: 1 metered history entry has no billed native request within 5 minutes of its time in any accepted session (Squad Scribe.md x1); usage that no accepted session recorded for it is not in the exact totals, and nothing is estimated for it.
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
| Dispatches (excluding Scribe) | 4 | derived |
| Rework dispatches | 0 | derived |
| First-pass yield | n/a | derived |
| Exact credits per accepted deliverable | n/a | measured (lower bound) |
| Rework share of exact credits | n/a | unavailable without per-history-run identity |
| Orchestration share of exact credits | 96.6% | measured (lower bound) |

Value is counted only for deliverables an independent review-class verdict accepted; a produced but unaccepted deliverable adds cost and no value. A review that names deliverables covers only those it names. Rework counts a repeat of the same agent and deliverable path only: a fix by a different agent, to a different file, or with no deliverable path is not counted, so the rework figures are a floor. Compare runs only for like work, using medians of several runs. Source: FinOps Foundation unit-economics KPIs (cost per unit of value, error and retry waste).

## Cost Comparison (exact native actual)

| Scenario | Basis | AI Credits | USD |
| -------- | ----- | ----------: | --: |
| With HVE Squad through 2026-11-23T20:07:09.169Z | complete deduplicated native records; coordinator and descendants included | 3013.95 | 30.1395 |
| Without HVE Squad: same exact role-request token categories on `Claude Sonnet 5.5 (copilot)`, no coordinator or Scribe | calculated counterfactual from exact request tokens | 136.92 | 1.3692 |

HVE Squad is **28.7703 USD (2101.2%) higher** than the same exact role traffic on `Claude Sonnet 5.5 (copilot)`.

> The HVE Squad row is exact as of the native session snapshot: 544 complete accounting records, cumulative digest `F3969F433ADCCAFA88AAD93BEA9E7F8337CB0AD23CE7772CE1C9C24287ABF7ED`. The without-Squad row is not an observed alternate run; it holds every exact role request's uncached input, cache read, cache write, and output counts constant, reprices them on one model, and excludes coordinator/Scribe overhead. It therefore measures routing savings on identical traffic and does not invent context growth, rework, or behavioral differences for a hypothetical manual run.

> Lower bound: the exact total leaves out usage that no accepted native session could account for (see the `NOTE:` lines in the Derivation block). Nothing was estimated to fill the gap.
