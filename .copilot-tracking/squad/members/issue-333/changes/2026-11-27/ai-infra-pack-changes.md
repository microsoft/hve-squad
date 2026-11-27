<!--
Copyright (c) Microsoft Corporation.
Licensed under the MIT license.
-->
<!-- markdownlint-disable-file -->
# RPI Changes: AI-Ready Infrastructure and AI Landing Zones Pack (`issue-333` — `D04` / `P03`)

## Metadata

* Task ID: `issue-333` (`microsoft/hve-squad#333`)
* Related plan: [.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md](../../plans/ai-infra-pack-plan.md)
* Implementation date: `2026-11-27`

## Execution Status

* Status: Complete (for declared scope `P03` / `D04`)
* Declared invocation scope: `P03` (`P03-T01`, `P03-T02` — deliverable `D04`)
* Completed scope markers: `P03`, `P03-T01`, `P03-T02` (alongside previously landed `P01-T01`, `P01-T02`, `P02-T01`, `P02-T02`, `P02-T03`, and `P04-T01`)
* All remaining active-plan markers: `P04-T02`, `P04-T03` (`D06`), `P05` (`P05-T01` — `D07`)
* Status basis: All `P03-T01` and `P03-T02` requirements and Council conditions hold, all `D04` artifacts are written, the repository-wide pack enumeration audit is complete, and all plugin build, PSScriptAnalyzer, source header, Squad Studio, and documentation release/fragment checks pass.

## Execution Summary

Completed deliverable `D04` (`P03-T01`, `P03-T02`) for `microsoft/hve-squad#333`:
* Added `ai-infra` to the generated `squad-run` invocation skill's `**pack** (optional)` enumeration in [scripts/Build-SquadPlugin.ps1](../../../../../../scripts/Build-SquadPlugin.ps1) and verified runtime compatibility with `Get-SquadInitContext.ps1`, `SquadCoordinator.psm1` (`Get-SquadPackResourceInventory`), and `Build-SquadPlugin.ps1` without production coordinator script changes (`P03-T01`).
* Added `ai-infra` (`hasPreview: false`, `previewNote: ''`, zero hard-coded prices or SKUs) to exported `PACK_CARDS` in [squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs) and listed `ai-infra` in [squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md) (`P03-T02`).
* Created [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md) matching the `data-release-change="20261127-ai-infra-pack"` marker in [docs/usage.html](../../../../../../docs/usage.html) and [docs/fr/usage.html](../../../../../../docs/fr/usage.html) and verified `scripts/docs-release.mjs` (`readReleaseInfo().pendingChanges`), `npm run docs:check`, and `npm run docs:test` (`P03-T02`).

## Completed Work

### Align Generated `squad-run` Pack Enumeration and Verify Runtime Compatibility

* Related phase or task: `P03-T01`
* Files:
  * [scripts/Build-SquadPlugin.ps1](../../../../../../scripts/Build-SquadPlugin.ps1)
* Behavior or functionality changed: The generated `squad-run` skill template (`skills/squad/invocations/squad-run/SKILL.md` in built plugins) now includes `` `ai-infra` `` in its `**pack** (optional)` parameter list alongside the nine existing packs (`power-platform`, `m365-copilot`, `aws`, `foundry`, `fabric`, `dotnet`, `aks`, `azure-functions`, `azure-cloud`), matching the `Registered Packs` table in [squad-src/.github/instructions/squad/squad-roster.instructions.md](../../../../../../squad-src/.github/instructions/squad/squad-roster.instructions.md). Runtime verification confirmed [squad-src/.github/skills/squad/scripts/Get-SquadInitContext.ps1](../../../../../../squad-src/.github/skills/squad/scripts/Get-SquadInitContext.ps1) and [squad-src/.github/skills/squad/scripts/SquadCoordinator.psm1](../../../../../../squad-src/.github/skills/squad/scripts/SquadCoordinator.psm1) table-drive `ai-infra` discovery and deduplicate shared `azure-aigateway` (`packs: ["ai-infra"]` for `-Pack ai-infra` and `packs: ["foundry","ai-infra"]` for `-Pack foundry,ai-infra`), and `Build-SquadPlugin.ps1` copies both cold references (`ai-infra-blueprint.md` and `ai-infra-platform-options.md`) into the plugin distribution (`D7-1`).
* Validation: Passed (`Invoke-Pester` on [tests/tier0/Build-SquadPlugin.Tests.ps1](../../../../../../tests/tier0/Build-SquadPlugin.Tests.ps1), [tests/tier0/Packaging.Tests.ps1](../../../../../../tests/tier0/Packaging.Tests.ps1), and [tests/tier0/PackRegistry.Tests.ps1](../../../../../../tests/tier0/PackRegistry.Tests.ps1): `274 Passed, 0 Failed, 5 Skipped`; `Invoke-ScriptAnalyzer -Path ./scripts/Build-SquadPlugin.ps1 -Settings ./PSScriptAnalyzerSettings.psd1`: `0 Errors`).

### Expose `ai-infra` in HVE Squad Studio (`PACK_CARDS`, `README.md`) and Add Unreleased Release Fragment

* Related phase or task: `P03-T02`
* Files:
  * [squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs)
  * [squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md)
  * [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md)
* Behavior or functionality changed: Exported `PACK_CARDS` in [squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs) now includes the opt-in `ai-infra` card (`title: 'AI-Ready Infrastructure & Landing Zones'`, `hasPreview: false`, `previewNote: ''`, with no hard-coded prices or SKUs) so `detectSuggestedPacks`, `store.mjs`, `state-reader.mjs`, and `server.mjs` recognize and suggest `ai-infra` without selecting it by default, and [squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md) lists `ai-infra` in the Specialist Add-Ons overview. [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md) records the `bump: patch` / `type: Added` release fragment matching the `data-release-change="20261127-ai-infra-pack"` markers in [docs/usage.html](../../../../../../docs/usage.html) and [docs/fr/usage.html](../../../../../../docs/fr/usage.html).
* Validation: Passed (`node --test tests/tier0/squad-studio/studio.test.mjs`: `7 Passed, 0 Failed`; `npm run docs:check`: `PASS` across 28 pages; `npm run docs:test`: `194 Passed, 0 Failed`; `scripts/Test-SourceHeaders.ps1`: `Eligible=428 Headed=428 Excluded=190`, exit code `0`).

## Repository-Wide Pack Enumeration Audit (`azure-cloud` / Pack List Touch Sites)

Searched the entire repository for `azure-cloud` and sibling pack identifiers (`azure-functions`, `m365-copilot`, `power-platform`, `foundry`, `fabric`, `dotnet`, `aks`) across `scripts/`, `site/`, `squad-src/`, `apm.yml`, `package.json`, `.github/`, `docs/`, and `tests/`. Every pack enumeration site and its disposition is recorded below:

| Category | File / Surface | Disposition | Owner / Deliverable |
|---|---|---|---|
| **Plugin Builder Script** | [scripts/Build-SquadPlugin.ps1](../../../../../../scripts/Build-SquadPlugin.ps1) (`squad-run` skill `**pack** (optional)` bullet) | **Updated in `D04`** — added `` `ai-infra` `` so generated `squad-run` matches `squad-roster.instructions.md` | `developer` (`D04` / `P03-T01`) |
| **Squad Studio Catalog & Docs** | [squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/catalog.mjs) (`PACK_CARDS`) & [squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md](../../../../../../squad-src/.github/skills/squad/extensions/hve-squad-studio/README.md) | **Updated in `D04`** — added `ai-infra` (`hasPreview: false`) to `PACK_CARDS` and `README.md` | `developer` (`D04` / `P03-T02`) |
| **Squad Studio Consumers** | `squad-src/.github/skills/squad/extensions/hve-squad-studio/lib/{server,store,state-reader}.mjs` | **No edit needed (table-driven)** — dynamically read `PACK_CARDS` from `lib/catalog.mjs` | Verified in `D04` |
| **Unreleased Release Fragment** | [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md) | **Created in `D04`** — matches `data-release-change="20261127-ai-infra-pack"` in `docs/usage.html` and `docs/fr/usage.html` | `developer` (`D04` / `P03-T02`) |
| **Coordinator & Runtime Scripts** | `squad-src/.github/skills/squad/scripts/{SquadCoordinator.psm1,Get-SquadInitContext.ps1,Install-SquadPackResources.ps1,Get-SquadRosterExpansionPlan.ps1,Expand-SquadRoster.ps1}` | **No edit needed in `D04` (table-driven)** — parse `squad-roster.instructions.md`, `profiles-and-packs.md`, and `roster-catalog.md` dynamically | Verified in `D04` (`P03-T01`) |
| **Repo Build/Docs/Sync Scripts, `site/`, `apm.yml`, `package.json`, `.github/workflows/`** | `scripts/*.{ps1,mjs}` (excluding `Build-SquadPlugin.ps1`), `site/**`, `apm.yml`, `package.json`, `.github/workflows/*.yml` | **No edit needed** — zero static pack-list enumerations present | Verified in `D04` |
| **Cold References (`ai-infra` & `foundry`)** | `squad-src/.github/skills/squad/references/{ai-infra-blueprint,ai-infra-platform-options,foundry-blueprint,foundry-platform-options}.md` | **Already handled in `D01`** (`P01-T01`, `P01-T02`) | `prompt-engineer` (`D01`) |
| **Hot Instructions, Prompt, Init & Catalog Tables** | `squad-src/.github/instructions/squad/squad-{roster,routing,federation}.instructions.md`, `squad-src/.github/prompts/squad/squad.prompt.md`, `squad-src/.github/skills/squad/references/{coordinator-init,profiles-and-packs,roster-catalog}.md` | **Already handled in `D02`** (`P02-T01`, `P02-T02`) | `prompt-engineer` (`D02`) |
| **Specialist Agent Charters (7 files)** | `squad-src/.github/agents/squad/squad-{azure-architect,cost-manager,observability-planner,azure-diagnose,iac-author,deployer,asbuilt-author}.agent.md` | **Already handled in `D03`** (`P02-T03`); `squad-implementor.agent.md` and `squad-modernization-planner.agent.md` are not `ai-infra` roles | `prompt-engineer` (`D03`) |
| **Bilingual Usage Docs** | [docs/usage.html](../../../../../../docs/usage.html), [docs/fr/usage.html](../../../../../../docs/fr/usage.html) | **Already handled in `D05`** (`P04-T01`) | `technical-writer` (`D05`) |
| **Behavior Contract & Tier 0 / Tier 1 Suites** | [tests/squad-behavior-contract.md](../../../../../../tests/squad-behavior-contract.md), `tests/tier0/{PackRegistry,CoordinatorRuntime,ExpandSquadRoster,InitializeSquadFederation,WatchMode,Packaging}.Tests.ps1`, [tests/tier0/squad-studio/studio.test.mjs](../../../../../../tests/tier0/squad-studio/studio.test.mjs), [tests/tier1/Invoke-Tier1LiveRun.ps1](../../../../../../tests/tier1/Invoke-Tier1LiveRun.ps1) | **Out of scope for `D04` (assigned to `D06`)** — left untouched per `D04` task boundary | `developer` (`D06` / `P04-T02`, `P04-T03`) |

## Implementation-Time Plan Updates

* None (Council conditions from `implementation-brief.md` — using exported `PACK_CARDS` in `lib/catalog.mjs` and running `node --test tests/tier0/squad-studio/studio.test.mjs` without creating a second test file under `squad-src/` — were applied directly as binding implementation inputs).

## Business Alignment

* None

## Validation Record

| Check | Scope | Status | Evidence or reason |
|---|---|---|---|
| `pwsh -NoProfile -File ./scripts/Test-SourceHeaders.ps1` | `P03` | Passed | `Eligible=428 Headed=428 Excluded=190` (exit code `0`) |
| `Invoke-ScriptAnalyzer -Path ./scripts/Build-SquadPlugin.ps1 -Settings ./PSScriptAnalyzerSettings.psd1` | `P03-T01` | Passed | `0 Errors` (`4` pre-existing function-level warnings at lines `450`, `465`, `569`, `819`; `0` issues on modified `squad-run` block) |
| `Invoke-Pester` on `tests/tier0/Build-SquadPlugin.Tests.ps1`, `tests/tier0/Packaging.Tests.ps1`, and `tests/tier0/PackRegistry.Tests.ps1` | `P03-T01` | Passed | `274 Passed, 0 Failed, 5 Skipped` in `134.71s` (including `offers exactly the registered packs in the generated invocation` at `Build-SquadPlugin.Tests.ps1:463` and `keeps every pack aligned across roster, guidance, routing, prompt, federation, and docs` in `PackRegistry.Tests.ps1`) |
| `Get-SquadInitContext.ps1 -RepoRoot . -Pack ai-infra` & `-Pack foundry,ai-infra` | `P03-T01` | Passed | Discovers `ai-infra` with all 9 roles and returns `azure-aigateway` with `packs: ["ai-infra"]` (single) and `packs: ["foundry","ai-infra"]` (shared deduplicated) |
| `node --test tests/tier0/squad-studio/studio.test.mjs` | `P03-T02` | Passed | `7 Passed, 0 Failed` (`246.69ms`), plus `detectSuggestedPacks` verified to match `ai-infra` (`hasPreview: false`) |
| `npm run docs:check` (`node scripts/check-docs.mjs`) | `P03-T02` | Passed | `Documentation: 28 pages; source prose, technical examples, languages, local links and search indexes preserved.` |
| `npm run docs:test` (`node --test tests/docs/*.test.mjs`) | `P03-T02` | Passed | `194 Passed, 0 Failed` (`20,585.68ms`), including `release.test.mjs` fragment/badge parity checks with `20261127-ai-infra-pack` in `readReleaseInfo().pendingChanges` |

## Pre-Review Reconciliation

* Plan markers and task-local context: Current for `P03` (`P03-T01`, `P03-T02` complete; plan file kept read-only per coordinator brief).
* Completed-work entries and handoff prose: Current and reconciled with all 4 modified/created repository files.
* Validation, blockers, remaining work, and follow-up items: Current; all `D04` validations passed with zero blockers.
* Review readiness: Ready for `D06` (`P04-T02`, `P04-T03` test suite expansion) followed by `D07` (`P05-T01` `Squad Reviewer`).

## Blockers

* None

## Remaining Work

* `P04-T02`, `P04-T03` (`D06`, owned by `developer`): Expand [tests/squad-behavior-contract.md](../../../../../../tests/squad-behavior-contract.md), Tier 0 unit suites (`PackRegistry`, `CoordinatorRuntime`, `ExpandSquadRoster`, `InitializeSquadFederation`, `WatchMode`, `Packaging`, `studio.test.mjs`), and Tier 1 `pack-lifecycle` verification.
* `P05` (`P05-T01`, `D07`, owned by `tester` / `Squad Reviewer`): Execute full validation sequence and implementation-versus-plan conformance review.

## Follow-Up Items

* Canonical plan list: [.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md](../../plans/ai-infra-pack-plan.md), `## Follow-Up Items`
* None added by `D04`.

## Return-to-Caller State

* Implementation execution status: Complete for declared scope `P03` (`P03-T01`, `P03-T02` / deliverable `D04`); Partial across full plan (`P04-T02`, `P04-T03`, and `P05-T01` remain).
* Declared scope and markers: `P03` (`P03-T01`, `P03-T02` completed); remaining active-plan markers: `P04-T02`, `P04-T03`, `P05` (`P05-T01`).
* Validation coverage: All 7 verification checks for `D04` passed (`Test-SourceHeaders.ps1`, `PSScriptAnalyzer`, `Build-SquadPlugin.Tests.ps1`, `Packaging.Tests.ps1`, `PackRegistry.Tests.ps1`, `Get-SquadInitContext.ps1`, `studio.test.mjs`, `docs:check`, `docs:test`).
* Blockers: None.
* Current plan updates: None.
* Business alignment entries: None.
* Planning and critique state: Ready (`Go-With-Conditions` Council verdict + validated human decisions `D1`–`D3`).
* Follow-up items: None.
* Review readiness or no-handoff reason: `D04` (`P03`) is complete; full-plan review (`D07` / `P05-T01`) awaits `D06` (`P04-T02`, `P04-T03`).
* Continuation owner: Parent Squad Coordinator (`issue-333`).
