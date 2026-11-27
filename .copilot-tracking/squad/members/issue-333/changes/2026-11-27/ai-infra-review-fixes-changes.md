<!--
Copyright (c) Microsoft Corporation.
Licensed under the MIT license.
-->
<!-- markdownlint-disable-file -->
# RPI Changes: AI-Ready Infrastructure and AI Landing Zones Pack (`issue-333` — Closing Review Fixes `RV-001` & `RV-002`)

## Metadata

* Task ID: `issue-333` (`microsoft/hve-squad#333`)
* Related plan: [.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md](../../plans/ai-infra-pack-plan.md)
* Related review: [.copilot-tracking/squad/members/issue-333/reviews/2026-11-27/ai-infra-pack-review.md](../../reviews/2026-11-27/ai-infra-pack-review.md)
* Implementation date: `2026-11-27`

## Execution Status

* Status: Complete (for closing review findings `RV-001` and `RV-002`)
* Declared invocation scope: Closing review fixes `RV-001` (`P03-T02` / `D04`) and `RV-002` (`P04-T02` / `D06`)
* Completed scope markers: `P03-T02` (`RV-001`), `P04-T02` (`RV-002`)
* All remaining active-plan markers: `none` (`P01`–`P05` complete)
* Status basis: Both `Low`-severity review findings from [.copilot-tracking/squad/members/issue-333/reviews/2026-11-27/ai-infra-pack-review.md](../../reviews/2026-11-27/ai-infra-pack-review.md) (`RV-001` and `RV-002`) are resolved and verified against `npm run docs:test`, `scripts/Test-SourceHeaders.ps1`, `Invoke-ScriptAnalyzer` with `PSScriptAnalyzerSettings.psd1`, and `./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Lifecycle`.

## Execution Summary

Resolved both `Low`-severity closing review findings (`RV-001` and `RV-002`) recorded in [.copilot-tracking/squad/members/issue-333/reviews/2026-11-27/ai-infra-pack-review.md](../../reviews/2026-11-27/ai-infra-pack-review.md):
* **`RV-001` ([.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md)):** Corrected the blocklisted skill origin repository attributions on line 7 to match the verified origins recorded in [squad-src/.github/skills/squad/references/roster-catalog.md](../../../../../../squad-src/.github/skills/squad/references/roster-catalog.md) and [squad-src/.github/skills/squad/references/ai-infra-platform-options.md](../../../../../../squad-src/.github/skills/squad/references/ai-infra-platform-options.md): `Azure/aigateway` for `manage-ai-gateway` and `migrate-apim`, and `Azure-Samples/AI-Gateway` for `apim-bicep`, `apim-terraform`, `apim-policies`, and `apim-kql`.
* **`RV-002` ([tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1)):** Imported `SquadCoordinator.psm1` in `BeforeAll` and updated the partial-removal portion of the `ai-infra` test case (`SRC-27`, `PRF-30`) so that after writing the partially removed `team.md` (`Packs: ai-infra`), it extracts the remaining active pack list, exercises `Get-SquadPackResourceInventory` to verify the sibling `ai-infra` pack's shared `azure-aigateway` reach survives (`packs = @('ai-infra')`) while removed `foundry`-exclusive skills (`use-ai-gateway`) do not, and exercises `New-ExpansionPlanReady` and `Invoke-Expansion` to reapply `foundry` (matching the neighbouring `SRC-25` cold-removal+reapply pattern) while confirming `observability` remains in `alreadyPresentRoles`, `data-scientist,prompt-engineer,rai` are re-added, and `azure-aigateway` stays `declined`.

## Completed Work

### Correct Blocklisted Skill Origin Attributions in Unreleased Changelog Fragment (`RV-001`)

* Related phase or task: `P03-T02` (`D04` / `RV-001`)
* Files:
  * [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md)
* Behavior or functionality changed: Updated line 7 of [.changes/unreleased/20261127-ai-infra-pack.md](../../../../../../.changes/unreleased/20261127-ai-infra-pack.md) so the blocklisted skill repository attributions match [squad-src/.github/skills/squad/references/roster-catalog.md](../../../../../../squad-src/.github/skills/squad/references/roster-catalog.md) (`Azure/aigateway@feac191bd69d7a2fa4fdba5c233f3a1d1c40fc65` for `manage-ai-gateway`, `migrate-apim` and `Azure-Samples/AI-Gateway@98c242510ba53f4e83e5f4f19595907c8e0b36bc` for `apim-bicep`, `apim-terraform`, `apim-policies`, `apim-kql`) and [squad-src/.github/skills/squad/references/ai-infra-platform-options.md](../../../../../../squad-src/.github/skills/squad/references/ai-infra-platform-options.md) while keeping the release fragment frontmatter and bullet structure intact.
* Validation: Passed (`npm run docs:test`: `194 Passed, 0 Failed`; `pwsh -NoProfile -File ./scripts/Test-SourceHeaders.ps1`: `Eligible=428 Headed=428 Excluded=190`, exit code `0`).

### Exercise Production Inventory and Reapply Path After Partial Pack Removal (`RV-002`)

* Related phase or task: `P04-T02` (`D06` / `RV-002`)
* Files:
  * [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1)
* Behavior or functionality changed: Imported `SquadCoordinator.psm1` in `BeforeAll` of [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1) and expanded the partial-removal check inside the single focused `It` block (`expands ai-infra across azure and full, preserves declined azure-aigateway across sequential +foundry/+ai-infra expansion, and keeps sibling reach on partial removal (SRC-27, PRF-30)`) to parse the remaining `Packs:` line from `$afterPartial`, run `Get-SquadPackResourceInventory` to assert `azure-aigateway` reach survives with `packs = @('ai-infra')` while `foundry`-exclusive `use-ai-gateway` does not, and run `New-ExpansionPlanReady` + `Invoke-Expansion` to reapply `foundry` against the partially removed squad root (verifying `observability` in `alreadyPresentRoles`, `data-scientist,prompt-engineer,rai` in `addedRoles`, `azure-aigateway = declined`, and `Packs: ai-infra, foundry` restored).
* Validation: Passed (`Invoke-ScriptAnalyzer -Path ./tests/tier0/ExpandSquadRoster.Tests.ps1 -Settings ./PSScriptAnalyzerSettings.psd1`: `0 Errors`, `1 Warning` pre-existing at line 296, `11 Information` pre-existing; `$env:TIER0_BASE_SHA = (git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Lifecycle`: [tests/tier0/ExpandSquadRoster.Tests.ps1](../../../../../../tests/tier0/ExpandSquadRoster.Tests.ps1) `24 Passed, 0 Failed`; overall `Lifecycle` shard `210 Passed, 1 Failed, 3 Skipped`, where the single failure is the known pre-existing Windows-only `ScribeInitConvergence.Tests.ps1` failure).

## Implementation-Time Plan Updates

* None.

## Business Alignment

* None.

## Validation Record

| Check | Scope | Status | Evidence or reason |
|---|---|---|---|
| `npm run docs:test` (`node --test tests/docs/*.test.mjs`) | Docs & release fragment suite (`RV-001`) | `Passed` | `194 Passed, 0 Failed` in `21.98s` |
| `pwsh -NoProfile -File ./scripts/Test-SourceHeaders.ps1` | Repository source headers | `Passed` | `Eligible=428 Headed=428 Excluded=190` (exit code `0`) |
| `Invoke-ScriptAnalyzer -Path ./tests/tier0/ExpandSquadRoster.Tests.ps1 -Settings ./PSScriptAnalyzerSettings.psd1` | PowerShell static analysis (`RV-002`) | `Passed` | `0 Errors` (`1 Warning` and `11 Information` pre-existing in file) |
| `$env:TIER0_BASE_SHA=(git rev-parse origin/main); ./tests/tier0/Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Lifecycle` | Tier 0 `Lifecycle` shard (`RV-002`) | `Passed` (with 1 known pre-existing Windows failure) | `ExpandSquadRoster.Tests.ps1`: `24 Passed, 0 Failed` (`SRC-27, PRF-30` test passed in `69.15s`); overall `Lifecycle` shard: `210 Passed, 1 Failed, 3 Skipped` (known pre-existing Windows CRLF failure in `ScribeInitConvergence.Tests.ps1:741`: `keeps the brief below 8KB and makes success terminal without template reads`) |

## Pre-Review Reconciliation

* Plan markers and task-local context: `current`
* Completed-work entries and handoff prose: `current`
* Validation, blockers, remaining work, and follow-up items: `current`
* Review readiness: `ready`

## Blockers

* `none`

## Remaining Work

* `none`

## Follow-Up Items

* `none`

## Return-to-Caller State

* Implementation execution status: `Complete`
* Declared scope and markers: Closing review fixes `RV-001` (`P03-T02`) and `RV-002` (`P04-T02`); remaining active-plan markers: `none`
* Validation coverage: `npm run docs:test` (`194/194`), `scripts/Test-SourceHeaders.ps1` (`428/428`), `Invoke-ScriptAnalyzer` (`0` errors), `Invoke-Tier0Tests.ps1 -SourceRoot . -Shard Lifecycle` (`ExpandSquadRoster.Tests.ps1` `24/24` passed; `210` passed, `1` known pre-existing Windows failure in `ScribeInitConvergence.Tests.ps1`, `3` skipped)
* Blockers: `none`
* Current plan updates: `none`
* Business alignment entries: `none`
* Planning and critique state: `ready`
* Follow-up items: `none`
* Review readiness or no-handoff reason: `ready`
* Continuation owner: `parent` (`Squad Coordinator`)
