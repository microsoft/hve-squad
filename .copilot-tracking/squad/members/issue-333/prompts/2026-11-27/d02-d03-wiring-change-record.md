<!--
Copyright (c) Microsoft Corporation.
Licensed under the MIT license.
-->
<!-- markdownlint-disable-file -->
# D02 + D03 Change Record: Hot Instructions, Catalog Tables, Entrypoints & Specialist Charters (`P02-T01`..`P02-T03`)

* **Task ID:** `issue-333` (`microsoft/hve-squad#333`)
* **Deliverables:** `D02` (`P02-T01`, `P02-T02`) and `D03` (`P02-T03`), merged into one `prompt-engineer` dispatch per `implementation-brief.md`
* **Owner:** `prompt-engineer` (`Squad Prompt Engineer`)
* **Skill & Modes:** `hve-builder` (`improve,refactor,review,validate`)
* **Date:** `2026-11-27`

## 1. Files Modified & Measured Byte / Character Counts

Normalized byte counts were measured with `Get-SquadContractByteCount` (`tests/tier0/SquadPackage.psm1`); agent body character counts were measured with `Read-SquadArtifact` (`PKG-04`, 30,000-char ceiling).

### A. Budgeted Hot Files & Compact Reference (`P02-T01`)

| File Path | Baseline (`v0.18.1`) | Before (`HEAD`) | After (`D02`) | Net Delta | Remaining Allowance (Before) | Ceiling | Status |
|---|---:|---:|---:|---:|---:|---:|---|
| `squad-src/.github/instructions/squad/squad-roster.instructions.md` | `62,039 B` | `62,917 B` | `62,882 B` | **`-35 B`** | `+146 B` | `63,063 B` | **PASS** (`181 B` headroom left; all 5 locked normative sentences & `RQ-04` preserved) |
| `squad-src/.github/instructions/squad/squad-routing.instructions.md` | `29,893 B` | `29,887 B` | `29,979 B` | **`+92 B`** | `+262 B` | `30,149 B` | **PASS** (`170 B` headroom left) |
| `squad-src/.github/instructions/squad/squad-federation.instructions.md` | `41,418 B` | `41,621 B` | `41,633 B` | **`+12 B`** | `+53 B` | `41,674 B` | **PASS** (`41 B` headroom left) |
| `squad-src/.github/prompts/squad/squad.prompt.md` | `6,776 B` | `7,236 B` | `7,257 B` | **`+21 B`** | `+52 B` | `7,288 B` | **PASS** (`31 B` headroom left) |
| `squad-src/.github/skills/squad/references/coordinator-init.md` | `n/a` (`COORD-09`) | `7,436 B` | `7,614 B` | **`+178 B`** | `+756 B` | `8,192 B` | **PASS** (`578 B` headroom left) |

### B. Cold Catalog & Pack Reference Tables (`P02-T02`)

| File Path | Before (`HEAD`) | After (`D02`) | Net Delta | Raw On-Disk Bytes | Notes |
|---|---:|---:|---:|---:|---|
| `squad-src/.github/skills/squad/references/profiles-and-packs.md` | `29,322 B` | `32,776 B` | `+3,454 B` | `32,996 B` | Updated `last_updated: "2026-11-27"`, added `ai-infra` to `Registered Packs`, `Pack Resource Preflight`, `Pack Discovery`, `Seeded Selection Cues`, and preflight prose (`foundry` or `ai-infra` shared `azure-aigateway` isolation) |
| `squad-src/.github/skills/squad/references/roster-catalog.md` | `242,005 B` | `248,092 B` | `+6,087 B` | `248,845 B` | Updated `last_updated: "2026-11-27"`, updated `azure-architect` Cast Catalog row and `azure-aigateway` Prerequisites cell (`Verified On: 2026-11-15` preserved), added `ai-infra -> azure-aigateway` in `Registered Pack Skills`, added 4 `External Cast Blocklist` rows at verified 40-char SHAs, and added `#### AI-Ready Infrastructure and AI Landing Zones verification` |

`squad-src/.github/skills/squad/references/00-index.md`, `coordinator-runtime.md`, `federation-runtime.md`, `apm.yml`, and `tests/tier0/baselines/*.json` were **not** modified (`D7-5` Scribe cold core remains at `82,241 / 82,243 B`).

### C. Specialist Agent Charters (`P02-T03`)

| File Path | Added `ai-infra` Paragraph Chars (`<= 500` Cap) | `PKG-04` Body Chars (`<= 30,000` Cap) | Before / After Normalized Bytes | Notes |
|---|---:|---:|---:|---|
| `squad-src/.github/agents/squad/squad-azure-architect.agent.md` | **`485 chars`** | `14,803 / 30,000` | `13,766 B -> 14,803 B` (`+1,037 B`) | Added `ai-infra` bullet (`485 chars`), updated `foundry` shared `azure-aigateway` reach and upstream directive neutralization, and updated `developer` -> `iac-author` (`infra/{track}/{project}`) attribution at lines 24, 33, 86, 91, 114 |
| `squad-src/.github/agents/squad/squad-cost-manager.agent.md` | **`489 chars`** | `18,592 / 30,000` | `18,142 B -> 18,633 B` (`+491 B`) | Added `ai-infra` paragraph (`489 chars`) with full TCO roll-up, `CO:04`/`CO:06` idle-GPU scoping, no static rates, `indicative` labels, separate currencies, `confirm` tier, and Step 5 AI-spend separation |
| `squad-src/.github/agents/squad/squad-observability-planner.agent.md` | **`500 chars`** | `9,887 / 30,000` | `9,540 B -> 10,041 B` (`+501 B`) | Added `ai-infra` bullet (`500 chars`) for read-only DCGM GPU/VRAM, thermal/ECC/Xid, RDMA, Lustre/ANF, queue/TTFT, and Managed Prometheus/Monitor telemetry plans |
| `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md` | **`500 chars`** | `16,756 / 30,000` | `16,445 B -> 16,946 B` (`+501 B`) | Added `ai-infra` bullet (`500 chars`) for read-only GPU throttling, Xid/ECC, RDMA, Lustre/ANF, and APIM gateway triage with secrets redacted |
| `squad-src/.github/agents/squad/squad-iac-author.agent.md` | **`484 chars`** | `7,538 / 30,000` | `7,107 B -> 7,592 B` (`+485 B`) | Added `ai-infra` bullet (`484 chars`) for AVM-first `infra/{track}/{project}` conversion (`Not applicable` when advisory; passwordless/private-endpoint defaults) |
| `squad-src/.github/agents/squad/squad-deployer.agent.md` | **`494 chars`** | `12,001 / 30,000` | `11,520 B -> 12,015 B` (`+495 B`) | Added Step 2 item 7 (`494 chars`) keeping OIDC dry-run, `azure-deployment-preflight`, and Impactful-Action Gate while forbidding `az quota` writes, RI/SP/PTU purchases, and blocklisted skills |
| `squad-src/.github/agents/squad/squad-asbuilt-author.agent.md` | **`472 chars`** | `10,099 / 30,000` | `9,707 B -> 10,180 B` (`+473 B`) | Added `ai-infra` bullet (`472 chars`) for post-deploy read-only inventory, compliance, runbook, and DR documentation (`Not applicable` when advisory; never fabricate facts) |

## 2. Binding Council Conditions & Plan Requirements Checklist

| # | Binding Condition / Requirement (from `implementation-brief.md`, Council Findings & Plan `P02`) | Status | How & Where Satisfied |
|---|---|---|---|
| 1 | **Hot-file byte ceilings (`P02-T01`) & locked normative sentences/headings (`prefill-baseline.json`, `RQ-04`)** | `[x]` | `squad-roster.instructions.md` (`-35 B`), `squad-routing.instructions.md` (`+92 B`), `squad-federation.instructions.md` (`+12 B`), `squad.prompt.md` (`+21 B`), and `coordinator-init.md` (`+178 B`) all pass `HotFileBudget.Tests.ps1` and `Performance.Tests.ps1` (`85 Passed, 0 Failed`). |
| 2 | **Shared `azure-aigateway` reach prose & lifecycle edge cases across `roster-catalog.md`, `profiles-and-packs.md`, and `squad-azure-architect.agent.md`** | `[x]` | Updated `roster-catalog.md` (`azure-architect` Cast Catalog row, `azure-aigateway` `Registered External Cast` row keeping `Verified On: 2026-11-15` and substring ``Reach only while `foundry` is active``, and `Registered Pack Skills` `| ai-infra | azure-aigateway |`), `profiles-and-packs.md` (`Seeded Selection Cues`, `Pack Resource Preflight` table and `For foundry` / `For ai-infra` prose), and `squad-azure-architect.agent.md` (`foundry` and `ai-infra` bullets). |
| 3 | **Neutralize upstream `skills/azure-aigateway/SKILL.md` directives (`:22`, `:60`, `:75`, `:92-102`)** | `[x]` | Explicitly forbidden in `roster-catalog.md` (`azure-aigateway` `Prerequisites` cell) and `squad-azure-architect.agent.md` (`foundry` pack skill bullet), mandating packaged `auth-best-practices.md` (`ManagedIdentityCredential` / Entra ID + Azure RBAC). |
| 4 | **External Cast Blocklist entries at verified 40-char SHAs + separate excluded/unregistered labelling (`P02-T02`)** | `[x]` | Verified all 4 upstream SHAs via `gh api` (`feac191bd69d7a2fa4fdba5c233f3a1d1c40fc65`, `98c242510ba53f4e83e5f4f19595907c8e0b36bc`, `fd52bbbbe528843c0724dac32f76efb81cb52a71`, `a243267d0c7b75d25dba3a06c0819b5d86f115e0`), added the 4 rows to `External Cast Blocklist` in `roster-catalog.md` with exact failing gate steps and date `2026-11-27`, and documented `azure-compute`, `azure-storage`, `airunway-aks-setup`, `.github/plugins/azure-observability`, and whole-plugin forms as excluded/unregistered in `#### AI-Ready Infrastructure and AI Landing Zones verification`. |
| 5 | **7 specialist charter `ai-infra` paragraphs `<= 500` chars with `PKG-03` `references/` prefixes (`P02-T03`)** | `[x]` | Every added `ai-infra` paragraph is `<= 500` characters (`485`, `489`, `500`, `500`, `484`, `494`, `472` chars) and prefixes both references as `references/ai-infra-blueprint.md` and `references/ai-infra-platform-options.md`. Verified `PKG-03` (`SquadPackage.Tests.ps1`) resolves all 108 reference claims across all agents. |
| 6 | **`iac-author` attribution for `infra/{track}/{project}` conversion in `squad-azure-architect.agent.md`** | `[x]` | Updated `squad-azure-architect.agent.md` Purpose (`:24`), Governing Conventions (`:33`), Step 4 (`:86`, `:91`), and Required Protocol (`:114`) so Bicep/Terraform conversion under `infra/{track}/{project}` is attributed to `iac-author` rather than `developer`. |
| 7 | **`squad-cost-manager.agent.md` guards (`indicative`, `confirm`-tier, separate currencies, no static rates, Step 5 separation)** | `[x]` | Included in the `489`-char `ai-infra` paragraph in `squad-cost-manager.agent.md`; verified `0` matches against static-price regexes across all 7 charters and both references. |
| 8 | **Do NOT touch `references/00-index.md` (`D7-5`) or write squad state** | `[x]` | `00-index.md` and squad state files (`history/`, `decisions.md`, `state.json`, `consumption.md`) are untouched. |

## 3. Verification Commands and Results

1. **Source headers (`scripts/Test-SourceHeaders.ps1`):**
   * Command: `pwsh -NoProfile -File ./scripts/Test-SourceHeaders.ps1`
   * Result: **PASS** (`Eligible=428 Headed=428 Excluded=190`, exit code `0`).
2. **Hot-file budgets & normative sentence/heading inventory (`HotFileBudget.Tests.ps1`, `Performance.Tests.ps1`):**
   * Command: `$env:TIER0_BASE_SHA = (git rev-parse origin/main); Invoke-Pester -Container @(New-PesterContainer -Path './tests/tier0/HotFileBudget.Tests.ps1' -Data @{ SourceRoot = $PWD.Path }, New-PesterContainer -Path './tests/tier0/Performance.Tests.ps1' -Data @{ SourceRoot = $PWD.Path })`
   * Result: **PASS** (`85 Passed, 0 Failed, 0 Skipped`).
3. **Plugin packaging, `D7-1` reference delivery & `D7-5` Scribe cold core (`Packaging.Tests.ps1`):**
   * Command: `Invoke-Pester -Container @(New-PesterContainer -Path './tests/tier0/Packaging.Tests.ps1' -Data @{ SourceRoot = $PWD.Path })`
   * Result: **PASS** (`178 Passed, 0 Failed, 4 Skipped`).
4. **Installed-package `PKG-01..PKG-15` & `RQ-01..RQ-06` (`SquadPackage.Tests.ps1` via `Install-SquadPackage`):**
   * Command: `Install-SquadPackage -Destination $scratch -SourceRoot $PWD.Path -Target copilot` + `Invoke-Pester -Path './tests/tier0/SquadPackage.Tests.ps1'` (excluding `Advisory` tag, after fixing `auth-best-practices.md` phrasing in `squad-azure-architect.agent.md` so `PKG-03` does not treat an external skill's support file as a local `squad` skill reference)
   * Result: **PASS** (`706 Passed, 0 Failed, 1 Skipped`).
5. **Pack registry consistency (`PackRegistry.Tests.ps1`):**
   * Command: `Invoke-Pester -Container @(New-PesterContainer -Path './tests/tier0/PackRegistry.Tests.ps1' -Data @{ SourceRoot = $PWD.Path })`
   * Result: **15 Passed, 1 Expected Pending-D05 Failure** (the only failing assertion in `PackRegistry.Tests.ps1` is `<td(?:\s+[^>]*)?><code>ai-infra</code></td>` in `docs/usage.html` and `docs/fr/usage.html`, which belongs to parallel deliverable `D05` owned by `technical-writer`; all roster, mirror, guidance, routing, prompt, federation, `Pack Discovery`, `Registered External Cast`, `Registered Pack Skills`, `External Cast Blocklist`, and 90-day SHA backstop checks pass).
6. **Init context discovery & shared `azure-aigateway` deduplication (`Get-SquadInitContext.ps1`):**
   * Command: `pwsh -NoProfile -File ./squad-src/.github/skills/squad/scripts/Get-SquadInitContext.ps1 -RepoRoot . -Pack ai-infra` (and `-Pack foundry,ai-infra`, and no `-Pack`)
   * Result: **PASS** — discovers `{"pack":"ai-infra","roles":["architect","azure-architect","iac-author","deployer","cost-manager","observability","azure-diagnose","asbuilt-author","security"]}`, returns `azure-aigateway` with `packs: ["ai-infra"]` under `-Pack ai-infra`, deduplicates `azure-aigateway` with `packs: ["foundry","ai-infra"]` under `-Pack foundry,ai-infra`, and returns `packResources.Count = 0` when no `-Pack` is passed.

## 4. HVE Builder Review & Outcome Record

* **Active mode set:** `improve,refactor,review,validate`
* **Candidate revision reviewed:** Working tree after `P02-T01`, `P02-T02`, and `P02-T03` edits across all 14 target files.
* **Review verdict:** `Pass` (1 mechanical `PKG-03` finding caught during validation — `references/auth-best-practices.md` in `squad-azure-architect.agent.md` was parsed by `PKG-03` as a local `squad` skill reference claim — resolved in-place as `packaged auth-best-practices.md` and re-verified).
* **Validation result:** `Pass`
* **Overall outcome:** `Pass`
