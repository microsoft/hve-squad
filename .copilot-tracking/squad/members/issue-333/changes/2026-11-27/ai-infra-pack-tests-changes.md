<!--
Copyright (c) Microsoft Corporation.
Licensed under the MIT license.
-->
<!-- markdownlint-disable-file -->
# RPI Changes: AI-Ready Infrastructure and AI Landing Zones Pack Tests (`issue-333` — `D06` / `P04-T02`, `P04-T03`)

## Metadata

* Task ID: `issue-333` (`microsoft/hve-squad#333`)
* Related plan: [.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md](../../plans/ai-infra-pack-plan.md)
* Implementation date: `2026-11-27`

## Execution Status

* Status: Complete (for declared scope `D06` / `P04-T02`, `P04-T03`)
* Declared invocation scope: `P04-T02`, `P04-T03` (deliverable `D06`)
* Completed scope markers: `P04-T02`, `P04-T03` (completing `P04` alongside `P04-T01` / `D05`)
* All remaining active-plan markers: `P05` (`P05-T01` — `D07`, owned by `tester` / `Squad Reviewer`)
* Status basis: All `D06` requirements (`P04-T02`, `P04-T03`) and binding Council conditions hold; all 8 target test/contract files are updated; all edited suites, `studio.test.mjs`, `npm run docs:test`, `scripts/Test-SourceHeaders.ps1`, and `PSScriptAnalyzer` pass.

## Execution Summary

Completed deliverable `D06` (`P04-T02`, `P04-T03`) for `microsoft/hve-squad#333`:
* Added `SRC-26`, `SRC-27`, and `PRF-30` contract rows to [tests/squad-behavior-contract.md](../../../../../../tests/squad-behavior-contract.md) covering `ai-infra` role seeding, shared `azure-aigateway` isolation, sequential-expansion `declined` preservation, partial-removal sibling survival, five runtime evidence gates, zero static price/SKU tables, Advisory/PSA vs. `iac-author` (`infra/{track}/{project}`) and gated `deployer`/`asbuilt-author` closure, and `networking`/`identity` escalation.
* Extended [tests/tier0/PackRegistry.Tests.ps1](../../../../../../tests/tier0/PackRegistry.Tests.ps1), [tests/tier0/CoordinatorRuntime.Tests.ps1](../../../../../../tests/tier0/CoordinatorRuntime.Tests.ps1), [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1), [tests/tier0/InitializeSquadFederation.Tests.ps1](../../../../../../tests/tier0/InitializeSquadFederation.Tests.ps1), [tests/tier0/WatchMode.Tests.ps1](../../../../../../tests/tier0/WatchMode.Tests.ps1), [tests/tier0/Packaging.Tests.ps1](../../../../../../tests/tier0/Packaging.Tests.ps1), [tests/tier0/squad-studio/studio.test.mjs](../../../../../../tests/tier0/squad-studio/studio.test.mjs), and [tests/tier1/Invoke-Tier1LiveRun.ps1](../../../../../../tests/tier1/Invoke-Tier1LiveRun.ps1) with focused `ai-infra` test assertions.

## Completed Work

### Expand Behavior Contract and Tier 0 Unit Suites for `ai-infra`

* Related phase or task: `P04-T02`
* Files:
  * [tests/squad-behavior-contract.md](../../../../../../tests/squad-behavior-contract.md)
  * [tests/tier0/PackRegistry.Tests.ps1](../../../../../../tests/tier0/PackRegistry.Tests.ps1)
  * [tests/tier0/CoordinatorRuntime.Tests.ps1](../../../../../../tests/tier0/CoordinatorRuntime.Tests.ps1)
  * [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1)
  * [tests/tier0/InitializeSquadFederation.Tests.ps1](../../../../../../tests/tier0/InitializeSquadFederation.Tests.ps1)
  * [tests/tier0/WatchMode.Tests.ps1](../../../../../../tests/tier0/WatchMode.Tests.ps1)
  * [tests/tier0/Packaging.Tests.ps1](../../../../../../tests/tier0/Packaging.Tests.ps1)
  * [tests/tier0/squad-studio/studio.test.mjs](../../../../../../tests/tier0/squad-studio/studio.test.mjs)
* Behavior or functionality changed:
  * [tests/squad-behavior-contract.md](../../../../../../tests/squad-behavior-contract.md): Added `SRC-26`, `SRC-27`, and `PRF-30` documenting `ai-infra` role reuse, shared `azure-aigateway` isolation, blocklisted vs. excluded upstream candidates, five runtime evidence gates, Advisory/PSA vs. implementation closure, and profile role deltas.
  * [tests/tier0/PackRegistry.Tests.ps1](../../../../../../tests/tier0/PackRegistry.Tests.ps1): Added `Describe 'AI-Ready Infrastructure and AI Landing Zones official pack'` with two `It` blocks:
    1. `reuses existing roles, shares the single canonical azure-aigateway entry with foundry with zero preview skills, and blocklists failing candidates (SRC-26, PRF-30)`: Verifies `ai-infra` role parity (`architect,asbuilt-author,azure-architect,azure-diagnose,cost-manager,deployer,iac-author,observability,security`), profile role deltas (`full`, `azure`, `architecture`, `operations`, `default`), single canonical `azure-aigateway` row (`968f7bfaa01672e782dae2c6bab4fb9505ae047a`, `Verified On: 2026-11-15`) mapped to `ai-infra,foundry` with zero preview skills, blocklisted candidates at verified 40-char SHAs, and unblocked/excluded candidate separation.
    2. `enforces blueprint evidence gates, security/advisory boundaries, charter reach guards, and zero static price or SKU tables across references and charters (SRC-27)`: Verifies Gate 5 (`Rate Basis, Currency & Commitment Scope`), Advisory/PSA track (`Not applicable (advisory scope)`), `iac-author` (`infra/{track}/{project}`), unselectable `networking`/`identity` escalation, security/redaction/directive-neutralization clauses, `PKG-03` `references/ai-infra-*.md` pointers across all 7 modified charters, static-price regex check (`\$\d|\b(?:USD|EUR|GBP)\s*\d|\d+\.\d+\s*/\s*(?:hr|hour|month|mo|gpu|vcpu)\b`) across both references and all 7 modified charters, and bilingual `#ai-infra-setup` docs anchors.
  * [tests/tier0/CoordinatorRuntime.Tests.ps1](../../../../../../tests/tier0/CoordinatorRuntime.Tests.ps1): Added `deduplicates shared azure-aigateway across foundry and ai-infra, honors declined over host visibility, and previews ai-infra installs (SRC-26, SRC-27)` testing all 4 pack combinations (`@()`, `@('ai-infra')`, `@('foundry')`, `@('foundry', 'ai-infra')`), confirming `azure-aigateway` is listed once with `packs = @('foundry', 'ai-infra')`, `Assert-SquadPackResourceDecisions` preserves `declined` when `-AvailableSkill @('azure-aigateway')` is passed, and `Install-SquadPackResources.ps1 -Pack ai-infra -Confirmed -WhatIf` plans the pinned directory install without mutating the repo.
  * [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1): Added `ai-infra` default `resourceDecisions` (`azure-aigateway = 'declined'`) in `New-ExpansionPlan` and added `expands ai-infra across azure and full, preserves declined azure-aigateway across sequential +foundry/+ai-infra expansion, and keeps sibling reach on partial removal (SRC-27, PRF-30)` testing `azure +ai-infra` (adds `observability`), sequential `+ai-infra` then `+foundry` and `+foundry` then `+ai-infra` with `availableSkills = @('azure-aigateway')` (preserving `azure-aigateway=declined`), and partial pack removal keeping shared roles and sibling pack reach intact.
  * [tests/tier0/InitializeSquadFederation.Tests.ps1](../../../../../../tests/tier0/InitializeSquadFederation.Tests.ps1): Extended `requires pack resource preflight for an existing federation expansion member` to verify `ai-infra` member pack resource preflight (`azure-aigateway = 'declined'`, `packResources.Count = 1`) without mutating sibling sub-squads.
  * [tests/tier0/WatchMode.Tests.ps1](../../../../../../tests/tier0/WatchMode.Tests.ps1): Extended `RV3-002 accepts default, non-default, and profile-plus-pack rosters and rejects incomplete or extra roles` to assert `$catalog.PackSkills['ai-infra'] | Should -HaveCount 1` and verify Watch Mode fails closed (`^pack-skill-not-visible:ai-infra:`) when `azure-aigateway` is not host-visible and succeeds when visible.
  * [tests/tier0/Packaging.Tests.ps1](../../../../../../tests/tier0/Packaging.Tests.ps1): Extended `D7-1` `It '<_> ships in the built plugin output'` to assert `ai-infra-blueprint.md` and `ai-infra-platform-options.md` ship in the built plugin while `D7-5` Scribe cold core remains within budget.
  * [tests/tier0/squad-studio/studio.test.mjs](../../../../../../tests/tier0/squad-studio/studio.test.mjs): Imported `PACK_CARDS` and asserted `ai-infra` exists with `hasPreview === false` and `previewNote === ''`, is not selected by default in `composeStudioLaunch`, and is suggested by `detectSuggestedPacks` for AI Landing Zone / InfiniBand RDMA / GPU cluster / Managed Lustre prompts.
* Validation: Passed (`PackRegistry.Tests.ps1`: `27 Passed, 0 Failed`; `CoordinatorRuntime.Tests.ps1`, `ExpandSquadRoster.Tests.ps1`, `InitializeSquadFederation.Tests.ps1`, `WatchMode.Tests.ps1`, `Packaging.Tests.ps1`: `0 Failed`; `node --test tests/tier0/squad-studio/studio.test.mjs`: `7 Passed, 0 Failed`).

### Verify Tier 1 `pack-lifecycle` Parameterization for `ai-infra` and Backwards Compatibility

* Related phase or task: `P04-T03`
* Files:
  * [tests/tier1/Invoke-Tier1LiveRun.ps1](../../../../../../tests/tier1/Invoke-Tier1LiveRun.ps1)
* Behavior or functionality changed: Added `.EXAMPLE ./Invoke-Tier1LiveRun.ps1 -SourceRoot . -Scenario pack-lifecycle -Pack ai-infra -ProvisionOnly` to [tests/tier1/Invoke-Tier1LiveRun.ps1](../../../../../../tests/tier1/Invoke-Tier1LiveRun.ps1) and verified locally that `Get-RegisteredPackData` resolves `ai-infra` (`architect,azure-architect,iac-author,deployer,cost-manager,observability,azure-diagnose,asbuilt-author,security`), `scenarios/pack-lifecycle.json` substitutes `${pack}` -> `ai-infra` (`id: pack-lifecycle-ai-infra`), `Get-SquadInitContext.ps1 -RepoRoot .` with no `-Pack` returns `11` profiles and `0` `packResources`, and `git diff -- tests/tier0/baselines/ apm.yml squad-src/.github/skills/squad/references/00-index.md` is empty (`0` bytes). Live network `apm install` provisioning under `Invoke-Tier1LiveRun.ps1 -ProvisionOnly` was not executed per caller instruction (`do NOT run live network provisioning unless it is local-only`).
* Validation: Passed (`Get-SquadInitContext.ps1` no-pack check, baseline diff check, and `pack-lifecycle.json` + `Get-RegisteredPackData` local validation all passed).

## Implementation-Time Plan Updates

* None

## Business Alignment

* None

## Validation Record

| Check | Scope | Status | Evidence or reason |
|---|---|---|---|
| `$env:TIER0_BASE_SHA=(git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Coordinator -Output Normal` | `P04-T02` | Passed | `206 Passed, 0 Failed, 0 Skipped` (`EconomyRouting`, `SetSquadModelRouting`, `GetSquadDispatchBrief`, `CoordinatorRuntime`, `QuickDepth`, `AgentDiscovery`) |
| `$env:TIER0_BASE_SHA=(git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Lifecycle -Output Normal` | `P04-T02` | Passed (all touched suites); 1 pre-existing Windows CRLF failure in untouched `ScribeInitConvergence.Tests.ps1` | `210 Passed, 1 Failed, 3 Skipped` — `InitializeSquadFederation.Tests.ps1`, `ExpandSquadRoster.Tests.ps1`, `ConsumptionRatesSeeder.Tests.ps1`, `PromoteSquadFederation.Tests.ps1`, and `RegisterSquadConnector.Tests.ps1` all passed `100%`; only untouched `Deterministic fresh Init.keeps the brief below 8KB and makes success terminal without template reads` (`ScribeInitConvergence.Tests.ps1:741`) reported `8060` vs `8000` raw CRLF bytes on Windows (`scribe-init.md` is `7928` LF bytes on Linux) |
| `$env:TIER0_BASE_SHA=(git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Contracts -Output Normal` | `P04-T02` | Passed (all touched suites); 10 pre-existing Pester 5.7.1 float-precision failures in untouched `SquadUsage.Tests.ps1` / `GovernanceReport.Tests.ps1` | `1910 Passed, 10 Failed, 7 Skipped, 57 NotRun` — `SquadPackage.Tests.ps1`, `PackRegistry.Tests.ps1` (`27/27`), `Build-SquadPlugin.Tests.ps1`, `Performance.Tests.ps1`, `Packaging.Tests.ps1`, `WatchMode.Tests.ps1`, `HotFileBudget.Tests.ps1`, `SquadStudioCanvas.Tests.ps1`, and `SquadInstall.Tests.ps1` all passed `100%`; the 10 failures are pre-existing Pester 5.7.1 `[double]` equality comparisons in untouched `SquadUsage.Tests.ps1` (2) and `GovernanceReport.Tests.ps1` (8) |
| `$env:TIER0_BASE_SHA=(git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -PackageRoot <installed> -Shard Handoff -Output Normal` | `P04-T02` | Passed | `138 Passed, 0 Failed, 0 Skipped` (`WriteSquadHandoff.Tests.ps1`) |
| `node --test tests/tier0/squad-studio/studio.test.mjs` | `P04-T02` | Passed | `7 Passed, 0 Failed` (`235.78ms`) |
| `npm run docs:test` | `P04-T02` | Passed | `194 Passed, 0 Failed` (`21,101.83ms`) |
| `pwsh -NoProfile -File ./scripts/Test-SourceHeaders.ps1` | `P04-T02`, `P04-T03` | Passed | `Eligible=429 Headed=429 Excluded=190` (exit code `0`) |
| `PSScriptAnalyzer` (`-Settings ./PSScriptAnalyzerSettings.psd1`) on all 7 edited `.ps1` files | `P04-T02`, `P04-T03` | Passed | `0 Errors` across all 7 edited `.ps1` files (`PackRegistry.Tests.ps1`, `CoordinatorRuntime.Tests.ps1`, `ExpandSquadRoster.Tests.ps1`, `InitializeSquadFederation.Tests.ps1`, `WatchMode.Tests.ps1`, `Packaging.Tests.ps1`, `Invoke-Tier1LiveRun.ps1`); zero warnings or informational items on any newly added lines |
| Local Tier 1 `pack-lifecycle` `-Pack ai-infra` & backwards-compatibility checks | `P04-T03` | Passed | `Get-SquadInitContext.ps1 -RepoRoot .` returns `11` profiles and `0` `packResources`; `git diff -- tests/tier0/baselines/ apm.yml squad-src/.github/skills/squad/references/00-index.md` is empty; `Get-RegisteredPackData` resolves `ai-infra` (`9` roles) and `pack-lifecycle.json` renders `pack-lifecycle-ai-infra` |

## Pre-Review Reconciliation

* Plan markers and task-local context: Current for `D06` (`P04-T02`, `P04-T03` complete; plan file kept read-only per coordinator brief).
* Completed-work entries and handoff prose: Reconciled with all 8 modified test/contract files and 1 new change record.
* Validation, blockers, remaining work, and follow-up items: Current; zero blockers.
* Review readiness: Ready for `P05` (`P05-T01` / `D07` `Squad Reviewer` conformance review).

## Blockers

* None

## Remaining Work

* `P05` (`P05-T01` — `D07`, owned by `tester` / `Squad Reviewer`): Execute implementation-versus-plan conformance review and write `.copilot-tracking/squad/members/issue-333/reviews/2026-11-27/ai-infra-pack-review.md`.

## Follow-Up Items

* Canonical plan list: [.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md](../../plans/ai-infra-pack-plan.md), `## Follow-Up Items`
* None added by `D06`.

## Return-to-Caller State

* Implementation execution status: Complete for declared scope `D06` (`P04-T02`, `P04-T03`); Partial across full plan (`P05-T01` / `D07` review remains).
* Declared scope and markers: `P04-T02`, `P04-T03` (`D06`) completed; remaining active-plan marker: `P05` (`P05-T01`).
* Validation coverage: All 4 Tier 0 shards (`Coordinator`, `Lifecycle`, `Contracts`, `Handoff`), `studio.test.mjs`, `npm run docs:test`, `scripts/Test-SourceHeaders.ps1`, `PSScriptAnalyzer`, and local Tier 1 `pack-lifecycle` / backwards-compatibility checks executed and recorded.
* Blockers: None.
* Current plan updates: None.
* Business alignment entries: None.
* Planning and critique state: Ready (`Go-With-Conditions` Council verdict + validated human decisions `D1`–`D3`).
* Follow-up items: None.
* Review readiness or no-handoff reason: Ready for `Squad Reviewer` (`D07` / `P05-T01`).
* Continuation owner: Parent Squad Coordinator (`issue-333`).

