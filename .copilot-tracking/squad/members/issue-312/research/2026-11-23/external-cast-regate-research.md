---
title: External Cast re-gate research for issue 312
description: Evidence for re-gating four azure-diagnose rows from microsoft/microsoft-sql after the November 2026 upstream layout change
author: Squad Researcher
ms.date: 2026-11-23
ms.topic: reference
---

## Scope

Issue `Peter-N91/hve-squad#312` escalated four `azure-diagnose` rows whose `head_path_missing` hazard blocked silent renewal. This artifact is research only. It contains no plan and no production edits.

Background only (does not change the investigation): Verified On 2026-11-16, Age 7 days, 152 registered rows, 60-day renewal window with a 90-day backstop.

Licensing posture: upstream `SKILL.md` content is MIT (`Copyright (c) Microsoft Corporation`). Quotes below are limited to short frontmatter fragments; everything else is paraphrased.

## Repository evidence read

* `roster-catalog.md` lines 246 to 249 register the four rows, each pinned to `eeb1c6867c2d128763516a1aad41671c593cc189`, MIT, opt-in, `vendor-official`. Line 250 registers `read-execution-plan`, which is not escalated; its path still exists at HEAD in `plugins/microsoft-sql/skills/read-execution-plan/SKILL.md`.
* Verification Gate (line 438 onward) has eight ordered checks: frontmatter at origin, `disable-model-invocation`, `tools:` versus local tools, delegated-worker contract, collision, demand, runtime prerequisite, licence and origin. Outcome is Accepted (registered row) or Rejected (blocklist row citing the failing step).
* External Cast Blocklist (line 456 onward) is a table with columns Resource, Kind, Reason, Recorded On.
* Line 551 narrative records the `eeb1c68` pass: root LICENSE verbatim MIT, and thirteen Azure SQL Database skills passed disposable APM directory install with self-contained `references/`, no hooks, no MCP declarations.
* Old Prerequisites cells cite `references/query-store-queries.md`, `references/blocking-visibility-and-deadlock-graphs.md`, `references/dmv-and-limits-reference.md`, and `references/error-codes.md`, plus read-only `sqlcmd`.
* `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md` also names the old skills; Stage 2 must check it.

## Upstream facts

* HEAD `3224561abf68c98c1ab47e27d2a06e30acdd330a` (PR #90, 2026-11-17). Old paths under `plugins/microsoft-sql/skills/diagnose-*` return 404 there.
* Root `LICENSE` at HEAD begins `Microsoft SQL agent skills`, then `MIT License`, then `Copyright (c) Microsoft Corporation.` This matches the prior verbatim-MIT posture. Only the header was read; a full diff against the pinned SHA was not run.
* `plugins/microsoft-sql-admin/` holds `.mcp.json`, `mcp.json`, `mcp-installation.json`, `plugin.json`, `README.md`, and `skills/`. The plugin bundles a SQL Tools MCP server (`mssql_list_database_connections`, `mssql_connect_database`, `mssql_execute_query`). A directory-path skill install does not install that server.

## Old skill scope (pinned `eeb1c68`)

Recovered from the `description` fields:

| Old skill | Scope |
|---|---|
| `diagnose-slow-query` | Azure SQL Database. Triages into volatile, blocked, regressed, or growing via Query Store; "Ends in a diagnosis, not a fix" |
| `diagnose-blocking-and-deadlocks` | Live blocking chain plus database-scoped Extended Events deadlock graphs, with grants differing on Basic, S0, S1, and elastic pools |
| `diagnose-resource-pressure` | CPU, data and log IO, memory, and worker or session limits (10928, 10929, 10936) in one skill |
| `diagnose-connection-errors` | Pre-credential refusals by layer: firewall 40615, VNet 40914, public access 47073 and 42101, gateway 40532 and 40531, TLS 47072; excludes login failures |

## Successor candidates (HEAD `3224561`)

All under `plugins/microsoft-sql-admin/skills/<name>/SKILL.md`. Frontmatter read at origin; descriptions quoted minimally.

| Candidate | `description` excerpt | Other frontmatter |
|---|---|---|
| `slow-query-investigation` | "Use when one specific query got slow, changed plans, or swings from seconds to minutes with the same parameters." | `argument-hint` |
| `check-live-blocking` | "Use when queries hang right now: who is blocking, the head blocker..." | none |
| `blocking-root-cause-analysis` | "Use when the user asks what caused blocking that already ended..." | none |
| `deadlock-analysis` | Deadlocks on SQL Server or Managed Instance; "For Azure SQL Database, use `deadlock-analysis-azuresql`." | `user-invocable: true` |
| `deadlock-analysis-azuresql` | Deadlocks, history, or prevention on Azure SQL Database or Fabric SQL Database | `user-invocable: true` |
| `investigate-sql-cpu-pressure` | "Use when CPU is high on SQL Server or Azure SQL..." | `argument-hint` |
| `investigate-sql-data-io-pressure` | "...PAGEIOLATCH waits, high read latency..." | `argument-hint` |
| `investigate-sql-log-io-pressure` | "...WRITELOG waits, high log write percent..." | `argument-hint` |
| `investigate-sql-memory-issues` | "...RESOURCE_SEMAPHORE, spill to tempdb, out-of-memory errors (701, 802, 8645), or PLE drops." | `argument-hint` |
| `connectivity-troubleshoot` | "Use when a client can't connect or gets a connection error (timeout, 53/10060/10061, SQLSTATE 08001, TLS, firewall 'IP not allowed', 4060)." | `user-invocable: true`, `argument-hint` |
| `authentication-diagnostics` | "Use when a login fails (18456...)... For network errors, use `connectivity-troubleshoot`." | `user-invocable: true` |

No candidate frontmatter declares `tools:`, `disable-model-invocation`, hooks, or an MCP server. Each body has a "Required tools" preference order: the plugin's SQL Tools MCP server, then any host SQL tool, then `sqlcmd`. That is a soft preference with a `sqlcmd` fallback, not a hard dependency. Bodies also point to a `sql-tools-mcp-setup` skill.

## Gate results

Steps: 1 frontmatter at origin, 2 `disable-model-invocation`, 3 `tools:`, 4 worker contract, 5 collision, 6 demand, 7 runtime prerequisite, 8 licence and origin.

| Candidate | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| `slow-query-investigation` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `check-live-blocking` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `deadlock-analysis-azuresql` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `deadlock-analysis` | pass | pass | pass | pass | pass | fail (SQL Server and MI scope) | n/a | pass |
| `blocking-root-cause-analysis` | pass | pass | pass | pass | pass | optional | pass, note | pass |
| `investigate-sql-cpu-pressure` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `investigate-sql-data-io-pressure` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `investigate-sql-log-io-pressure` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `investigate-sql-memory-issues` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `connectivity-troubleshoot` | pass | pass | pass | pass | pass | pass | pass, note | pass |
| `authentication-diagnostics` | pass | pass | pass | pass | fail (overlaps `entra-id-auth`) | fail (no demand) | n/a | pass |

Evidence and notes:

* Steps 2 to 4: grep of the candidate files found no `tools:` or `disable-model-invocation` key and no parent-artifact, lane, or cycle input. `argument-hint` values such as `[TopN]` are optional user parameters, not a worker contract.
* Step 5: once the four old rows are removed, no other `azure-diagnose` row covers these jobs; `read-execution-plan` is a separate job. `authentication-diagnostics` overlaps the registered `entra-id-auth` skill on `developer`, which is why the old connection-errors description excluded login refusal.
* Step 6: demand comes from the existing `azure-diagnose` role, which already reached the four old skills.
* Step 7: the references changed. Examples: `check-live-blocking/references/azure-sql/references/blocking-visibility-and-deadlock-graphs.md`, `connectivity-troubleshoot/references/azure-sql/references/error-codes.md`, and `references/queries.md` for the slow-query and IO skills. Every Prerequisites cell needs rewriting. Disposable APM directory installs were not run in this stage.
* Step 8: MIT, `microsoft/microsoft-sql` (`vendor-official`), new path, SHA `3224561abf68c98c1ab47e27d2a06e30acdd330a`.

Caveat: successors were read by frontmatter and targeted grep, not line by line (`authentication-diagnostics` is 1105 lines, `connectivity-troubleshoot` 757).

## Determination per row

| Escalated row | Determination | Recommended successor set | Confidence |
|---|---|---|---|
| `diagnose-slow-query` | Same persona, renamed and relocated, scope shifted (adds parameter sniffing, stale statistics, time-of-day; also covers SQL Server) | `slow-query-investigation` | Medium |
| `diagnose-blocking-and-deadlocks` | Same persona, split in two | `check-live-blocking` plus `deadlock-analysis-azuresql` | Medium-high |
| `diagnose-resource-pressure` | Retired with no 1:1 successor; split into four narrower skills. Worker and session limit errors (10928, 10929, 10936) and `MAXDOP` coverage unconfirmed | Four `investigate-sql-*` skills (cpu, data-io, log-io, memory), or record a gap | Medium |
| `diagnose-connection-errors` | Same persona, renamed and relocated | `connectivity-troubleshoot` | Medium-high |

## Blocklist assessment

* No recommended successor fails steps 1 to 5, and none lacks a `SKILL.md`. No MCP or `tools:` dependency is declared.
* The four old names were removed upstream rather than rejected by the gate. Stage 2 should drop their registered rows (pins are unreachable), not blocklist them.
* Blocklist candidates: `authentication-diagnostics` (step 5, overlap with `entra-id-auth`; optional ledger entry) and `deadlock-analysis` (step 6, wrong platform scope; optional).
* If Stage 2 declines the four `investigate-sql-*` skills, `diagnose-resource-pressure` should be recorded as retired with a blocklist-style note citing `head_path_missing`. If any adopted skill fails the disposable install, move that skill to the blocklist.

## Exact references for Stage 2

* Old pinned SHA: `eeb1c6867c2d128763516a1aad41671c593cc189`.
* New HEAD SHA: `3224561abf68c98c1ab47e27d2a06e30acdd330a`.
* New paths: `plugins/microsoft-sql-admin/skills/{slow-query-investigation,check-live-blocking,deadlock-analysis-azuresql,investigate-sql-cpu-pressure,investigate-sql-data-io-pressure,investigate-sql-log-io-pressure,investigate-sql-memory-issues,connectivity-troubleshoot}/SKILL.md`.
* Install form, matching existing rows: `apm install microsoft/microsoft-sql/plugins/microsoft-sql-admin/skills/<name>#3224561abf68c98c1ab47e27d2a06e30acdd330a --target copilot`.
* Update the line 551 narrative with the new gate SHA and the layout change. Check `squad-azure-diagnose.agent.md`; `apm.yml` and `docs` showed no match for the old names.
* `read-execution-plan` is unaffected by the layout change.

## Gaps

* Disposable APM install of each successor not run.
* Full LICENSE diff not run beyond the header.
* Old skills' full frontmatter beyond `name` and `description` not exhaustively inspected.
* `resource-usage-sqlserver` and `sql-tools-mcp-setup` not gated.

## Planning readiness

`ready-with-gaps`.
