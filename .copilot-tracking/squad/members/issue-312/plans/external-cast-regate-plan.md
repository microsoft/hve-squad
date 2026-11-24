---
title: External Cast re-gate plan for issue 312
description: Implementation plan for re-gating four drifted azure-diagnose rows from microsoft/microsoft-sql at upstream HEAD 3224561
author: Squad Lead
ms.date: 2026-11-23
ms.topic: reference
---

## Scope and inputs

Plan for `Peter-N91/hve-squad#312`. Grounded in `research/2026-11-23/external-cast-regate-research.md` (readiness `ready-with-gaps`). Plan only; no production file is edited by this stage.

* Old pin: `eeb1c6867c2d128763516a1aad41671c593cc189`. New pin: `3224561abf68c98c1ab47e27d2a06e30acdd330a`. Gate date and new `Verified On`: `2026-11-23`.
* Rows to replace live in `squad-src/.github/skills/squad/references/roster-catalog.md` (currently lines 246 to 249). `read-execution-plan` (line 250) and every other `microsoft/microsoft-sql` row are not escalated and keep `eeb1c68` and `2026-11-16`.
* New path form: `plugins/microsoft-sql-admin/skills/<name>/SKILL.md`. Install form: `apm install microsoft/microsoft-sql/plugins/microsoft-sql-admin/skills/<name>#3224561abf68c98c1ab47e27d2a06e30acdd330a --target copilot`.

## Per-row decisions

The upstream removed the four old names, so the gate did not reject them. Outcome (a) replaces each old row with accepted successor rows. Outcome (b), the blocklist, is used only for resources that fail the gate.

| Old row | Decision | New row(s) | Reason |
|---|---|---|---|
| `diagnose-slow-query` | (a) replace | `slow-query-investigation` | Gates 1 to 8 pass; medium confidence, same persona. Note the broader scope (also SQL Server, parameter sniffing, stale statistics) in the Prerequisites cell. |
| `diagnose-blocking-and-deadlocks` | (a) replace one row with two rows | `check-live-blocking`, `deadlock-analysis-azuresql` | Medium-high confidence; both pass. One old row becomes two new rows, because the catalog's row grain is one resource per row and the skills are separate installs. `deadlock-analysis` is not registered (step 6, SQL Server and MI scope). |
| `diagnose-resource-pressure` | (a) replace one row with four rows | `investigate-sql-cpu-pressure`, `investigate-sql-data-io-pressure`, `investigate-sql-log-io-pressure`, `investigate-sql-memory-issues` | All four pass gates 1 to 8. No 1:1 successor, so this is a split, not a rename. Coverage of `10928`/`10929`/`10936` and `MAXDOP` is unconfirmed: the Prerequisites cells must not claim it, and the charter keeps its inline read-only fallback for those. |
| `diagnose-connection-errors` | (a) replace | `connectivity-troubleshoot` | Medium-high confidence. `authentication-diagnostics` is not registered. |

No row moves to the blocklist for the four old names. Blocklist additions (optional ledger entries from the research, recorded on `2026-11-23`):

* `authentication-diagnostics` (skill): Gate step 5, overlaps registered `entra-id-auth` on `developer`; also step 6, no demand.
* `deadlock-analysis` (skill): Gate step 6, targets SQL Server and Managed Instance rather than Azure SQL Database; `deadlock-analysis-azuresql` is the registered match.

Row count: 4 removed, 8 added; the `microsoft/microsoft-sql` total goes from 17 to 21.

Alternate-Agent and Selection-Cue conventions: all new rows stay under role `azure-diagnose`, tier `opt-in`, kind `skill`, `vendor-official`, MIT, Dispatch cell `n/a` — skill, reached rather than dispatched. External Cast rows are not Alternate Agents in the Cast Catalog table, so no Cast Catalog table edit and no new Selection Cue is needed. The selection logic lives in the charter text (deliverable 5).

## Deliverables in dependency order

| # | Deliverable | Owner | Artifact path | Depends on | Batch |
|---|---|---|---|---|---|
| 1 | Row replacement and blocklist entries | `developer` | `squad-src/.github/skills/squad/references/roster-catalog.md` | none | A |
| 2 | Narrative paragraph update | `developer` | same file, `#### Microsoft SQL verification` (around line 551) | 1 | A (same file, sequential edit) |
| 3 | Docs sync decision | `developer` | none; no edit | none | n/a |
| 4 | `apm.yml` decision | `developer` | none; no edit | none | n/a |
| 5 | Charter edit | `developer` | `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md` (line 40) | 1 | B |
| 6 | Tier 0 test update | `developer` | `tests/tier0/PackRegistry.Tests.ps1` (around lines 1146 to 1180) | 1, 5 | B |
| 7 | Change fragment | `developer` | `.changes/unreleased/<generated>.md` | 1 to 6 | C |
| 8 | Verification | `tester` (Stage 5) | none | 1 to 7 | D |

Implement shape: `deliverable-fan-out` is not needed. Only `developer` owns artifacts, so Stage 4 is a single build in the order above.

### 1. roster-catalog.md rows

For each new row, copy the old row's column layout and make these changes:

* Resource: the successor name in backticks. Origin: `` `microsoft/microsoft-sql` (`vendor-official`), `plugins/microsoft-sql-admin/skills/<name>/SKILL.md` ``. Install: the new install form above. Licence `MIT`. Verified On `2026-11-23`.
* Prerequisites: keep the tail "Whole-plugin forms unregistered. Directory path only. Do not auto-install". Rewrite the front to the successor's real needs. Name the packaged reference actually present (confirm each path with `gh api` at the pin before writing it; the research gives `check-live-blocking/references/azure-sql/references/blocking-visibility-and-deadlock-graphs.md`, `connectivity-troubleshoot/references/azure-sql/references/error-codes.md`, and `references/queries.md` for the slow-query and IO skills). State: "SQL Tools MCP server preferred when the host provides it; falls back to a host SQL tool, then read-only `sqlcmd`; a disposable directory install does not bring the MCP server." Keep the read-only and recommend-not-execute guardrails from the old cells (for example no `sp_query_store_force_plan`, no firewall changes inline). Keep `VIEW DATABASE STATE` and the `##MS_ServerStateReader##` note on the blocking and deadlock rows only if the successor bodies confirm them.
* Resource-pressure rows: add that worker and session limit errors and `MAXDOP` are not covered by this skill; the charter's inline read-only steps apply.
* Insert rows in the order: slow-query, check-live-blocking, deadlock-analysis-azuresql, four `investigate-sql-*` (cpu, data-io, log-io, memory), connectivity-troubleshoot, then the untouched `read-execution-plan`.
* Gate precondition: before writing, the implementer runs the disposable APM directory install for each of the 8 successors (a research gap) and diffs the full root `LICENSE` at `3224561` against the pinned one. If any install fails or the licence differs, stop that row and take outcome (b): add a blocklist row (columns Resource, Kind, Reason, Recorded On) citing the failing step (step 7 for install or runtime failure, step 8 for licence), and drop it from the registered rows. Report it in the change record.
* Blocklist: append the two ledger rows above in the existing table format.

### 2. Narrative paragraph

The file has no dedicated re-gate narrative convention (searches for re-verification show only policy text at lines 146 and 449). Define one in the file's style: leave the `eeb1c68` paragraph intact as history, and append a sentence group to `#### Microsoft SQL verification` beginning "Re-gated 2026-11-23 at `3224561abf68c98c1ab47e27d2a06e30acdd330a`". It states: root `LICENSE` still verbatim MIT; upstream moved the Azure SQL Database skills from `plugins/microsoft-sql` to `plugins/microsoft-sql-admin` and retired the four `diagnose-*` names; the eight successors (list them by name, grouped by the old row) passed gates 1 to 8 and disposable directory install; `read-execution-plan` and the other rows remain pinned at `eeb1c68`; `authentication-diagnostics` and `deadlock-analysis` blocklisted; plugin bundles an MCP server that a directory install does not bring. Also edit the existing sentence that lists the four old names under `azure-diagnose` so it does not read as current: mark them "(replaced at `3224561`, see below)". Bump front-matter `last_updated` to `2026-11-23`.

### 3. EN/FR docs

No edit. The researcher's grep, and a check of `docs/usage.html` and `docs/fr/usage.html`, found no references to these four skill names, to `microsoft-sql`, or to either SHA. The only `azure-diagnose` mentions are role names in profile and pack tables, which do not change. Stage 4 must not touch `docs/`. Stage 5 re-greps `docs/` for the old names to confirm.

### 4. apm.yml

No skill or MCP entries need updating. `apm.yml` has no `microsoft-sql` or old-skill matches; its only related line is the dependency on `squad-azure-diagnose.agent.md` (line 216), a file path that does not change. `scripts/Update-ApmDependencies.ps1` repins hve-core dependencies and is not the mechanism for External Cast rows; do not run it. `Sync-ExternalCast.ps1` handles `apm.yml` renewals and is used only in dry-run at Stage 5.

### 5. squad-azure-diagnose.agent.md

Line 40 (the bullet "Official Azure SQL Database diagnostic skills") names the old skills. Replace the names, keeping each parenthetical scope, as follows:

* `diagnose-slow-query` becomes `slow-query-investigation` (keep Query Store volatility, regression, growth, and wait-category routing; add parameter sniffing and stale statistics).
* `diagnose-blocking-and-deadlocks` becomes `check-live-blocking` (lock-chain and idle open-transaction head-blocker DMVs) and `deadlock-analysis-azuresql` (database-scoped Extended Events deadlock XML).
* `diagnose-resource-pressure` becomes the four `investigate-sql-*` skills, each with its scope (CPU, `PAGEIOLATCH` data IO, `WRITELOG` log IO, memory grants and spills). Add that `10928`/`10929`/`10936` worker and session limits and `MAXDOP` are not covered by a registered skill, so run them from the charter's inline read-only steps.
* `diagnose-connection-errors` becomes `connectivity-troubleshoot`.
* Leave `read-execution-plan` and the `entra-id-auth` sentence unchanged. Add that the SQL Tools MCP server is preferred when available, with host SQL tool or read-only `sqlcmd` fallback. Keep the closing read-only guardrails.

### 6. PackRegistry.Tests.ps1

The test `sources SQL migration and Azure SQL skills from official microsoft/microsoft-sql and blocklists fredgis` asserts a fixed list, count 17, a single `eeb1c68` pin, a `microsoft-sql-migration|microsoft-sql` path regex, and `Verified On` `2026-11-16` for every row. Required changes:

* `$expectedSql`: remove the four old names; add the eight new names; `HaveCount 17` becomes `21`.
* Path regex: allow `microsoft-sql-admin` as a third plugin directory; the install regex likewise.
* Pin and date assertions: split by row. The eight new rows must carry `#3224561abf68c98c1ab47e27d2a06e30acdd330a` and `2026-11-23` under `plugins/microsoft-sql-admin`; the remaining rows keep `eeb1c68`, their existing plugin directories, and `2026-11-16`. Use a small name-to-expectation set rather than loosening to a wildcard.
* Blocklist assertions: keep `sql-migration-advisor@fredgis` and `eeb1c68`; add `authentication-diagnostics` and `deadlock-analysis`.
* Add: the old four names are absent from registered rows, and no `microsoft-sql-admin` row carries a `tools:` or MCP claim in Prerequisites beyond the fallback statement (optional).
* The charter-text assertions for `squad-sql-migration-advisor.agent.md` stay as-is. Search the file for any other test naming the four old names (only lines 1149 and 1150 matched) and for the roster-catalog row count.

### 7. Change fragment

Stage 4 runs, from the repository root:

```bash
pwsh scripts/New-ChangeFragment.ps1 -Type Changed -Bump patch -Title "Re-gate drifted azure-diagnose External Cast rows" -Body "Replaced the four azure-diagnose rows from microsoft/microsoft-sql that upstream removed (diagnose-slow-query, diagnose-blocking-and-deadlocks, diagnose-resource-pressure, diagnose-connection-errors) with eight successor skills from plugins/microsoft-sql-admin, re-gated at 3224561 and verified on 2026-11-23. Blocklisted authentication-diagnostics and deadlock-analysis, and updated the Squad Azure Diagnose charter and Tier 0 registry test. Closes #312."
```

Stage 4 does not edit `CHANGELOG.md` directly (RELEASE STATE convention). Plan stage did not run this.

## Council disposition

Stage 3 (Council) is skipped as not applicable. The `default`-profile roster carries no architect, security, cost-manager, product-owner, or rai role, and the work is a mechanical, read-only-upstream-driven catalog re-gate with no cross-domain architecture, security, cost, product, or RAI risk. This mirrors the Council Disposition in `.copilot-tracking/squad/members/issue-304/decisions.md`.

## Verification (Stage 5, `tester`)

* `./scripts/Sync-ExternalCast.ps1 -DryRun` completes with no `head_path_missing` hazard for the eight new rows and no unexpected drift for untouched rows.
* `Invoke-Pester -Path tests/tier0/PackRegistry.Tests.ps1` (smallest relevant selection; do not run the full Tier 0 suite).
* Grep for the old four names under `squad-src/`, `tests/`, `docs/`, `apm.yml`; expect matches only in the history narrative, the changelog fragment, and the research artifacts.
* Check the fragment exists under `.changes/unreleased/` with bump `patch`.

## Artifacts Stage 4 produces or Stage 5 checks

* `squad-src/.github/skills/squad/references/roster-catalog.md`
* `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md`
* `tests/tier0/PackRegistry.Tests.ps1`
* `.changes/unreleased/<generated fragment>.md`
* Change record `.copilot-tracking/squad/members/issue-312/changes/` and review `.copilot-tracking/squad/members/issue-312/reviews/`
* Not edited: `docs/usage.html`, `docs/fr/usage.html`, `apm.yml`, `CHANGELOG.md`

## Assumptions and open decisions

* Assumes the disposable installs and full licence diff pass; the fallback is outcome (b) per row.
* Assumes the Prerequisites reference paths hold at the pin; Stage 4 confirms each before writing.
* Open: whether to keep resource-pressure rows if coverage of worker limits is judged insufficient by the reviewer; the plan adopts them with the gap stated.

## Gaps in research

Disposable installs not run; full LICENSE diff not run; successors read by frontmatter and targeted grep only; `resource-usage-sqlserver` and `sql-tools-mcp-setup` not gated.
