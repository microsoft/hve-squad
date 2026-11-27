<!--
Copyright (c) Microsoft Corporation.
Licensed under the MIT license.
-->

# Council finding: challenger lens (issue #333)

Verbatim return of the Squad Challenger for the pre-implementation Council on `.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md`. Captured by the coordinator as lossless transport; the agent was findings-only and read-only. Dispatched with routed model `gemini-4-argon`; the agent's self-reported identity below is unverified by the host.
Verdict: Conditional
Risk: Medium

Blocking issues:
- None.

Conditions:
- **Fix parallel Batch 2 dependency hazard (`D04` vs. `D02`):** In `.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md:350-355`, `D04` (`P03-T01`/`P03-T02`) is scheduled in parallel with `D02` (`Batch 2C`, depending only on `D01`), contradicting `P03-T01`/`P03-T02` (`ai-infra-pack-plan.md:189,211`). `D04` must run **after** `D02` (or verify only after `D02` lands), because `Build-SquadPlugin.Tests.ps1:463` compares generated `squad-run` packs against `squad-roster.instructions.md` and `Get-SquadPackResourceInventory -Pack @('ai-infra')` (`SquadCoordinator.psm1:198`) throws `Unknown pack: ai-infra` until `D02` updates `squad-roster.instructions.md` and `roster-catalog.md`.
- **Remove non-existent Studio test path in `P03-T02` and `P05-T01`:** `ai-infra-pack-plan.md:206,328` runs `node --test squad-src/.github/skills/squad/extensions/hve-squad-studio/studio.test.mjs tests/tier0/squad-studio/studio.test.mjs`. Only `tests/tier0/squad-studio/studio.test.mjs` exists on disk; passing the non-existent `squad-src/.../studio.test.mjs` path causes `node --test` to fail.
- **Update two omitted pack-reach prose surfaces in `D02` (`P02-T02`):** (1) Update the `Cast Catalog` `azure-architect` row (`squad-src/.github/skills/squad/references/roster-catalog.md:27`), which currently states `azure-aigateway` applies only `When the Foundry pack is active`; (2) update the `Seeded Selection Cues` paragraph (`squad-src/.github/skills/squad/references/profiles-and-packs.md:57`) to include `ai-infra` alongside `foundry`, `fabric`, `dotnet`, `aks`, `azure-functions`, and `azure-cloud`.
- **Enforce `check-docs.mjs` structural/inline-code/link parity in `D05` (`P04-T01`):** Beyond `<pre><code>` blocks (`ai-infra-pack-plan.md:226`), `scripts/check-docs.mjs:52-63` asserts identical HTML tag sequences (`shape`), inline `<code>` arrays in exact order (`inline`), and external `https?://` link arrays (`externalLinks`) between `docs/usage.html` and `docs/fr/usage.html`.
- **Preserve locked baseline sentences, `Verified On: 2026-11-15`, and `D7-2` citation hygiene:** When compacting `squad-roster.instructions.md` (146 B left), `squad-federation.instructions.md` (53 B left), and `squad.prompt.md` (52 B left across two pack lists at lines 4 and 17 = +21 B), preserve all 7 normative sentences locked in `tests/tier0/baselines/prefill-baseline.json` (`Performance.Tests.ps1:382-450`) and `RQ-04` (`SquadPackage.Tests.ps1:174-184`). Keep `azure-aigateway` `Verified On` at `2026-11-15` (`PackRegistry.Tests.ps1:662`), and never write bare `.github/instructions/squad/` directory strings in new references/charters (`Packaging.Tests.ps1:124-131` `D7-2`; `Build-SquadPlugin.ps1:480-505` only rewrites `<name>.instructions.md`).
- **Require `D2` Option A or B (reject Option C) and separate unregistered vs. blocklisted skills:** To satisfy Issue #333 AC-2, register `azure-aigateway` under `ai-infra` (`Option A/B`). In `ai-infra-platform-options.md` (`ai-infra-pack-plan.md:108`), label `azure-compute`, `azure-storage`, `airunway-aks-setup`, and `azure-observability` as *unregistered/excluded* rather than *blocklisted* unless added to `External Cast Blocklist` (`PackRegistry.Tests.ps1:1007-1012`).

Suggested follow-ups:
- Explicitly note in `ai-infra-blueprint.md` that CycleCloud/Slurm, Managed Lustre/ANF, and DCGM have no dedicated upstream `SKILL.md` in audited repos (`ai-infra-pack-research.md:155,214-215`) and rely on official reference repos/Learn docs plus `telemetry-foundations` and composable `aks`/`azure-cloud` skills.
- Coordinate with `#278`, `#290`, `#298`, and `#299` once those issues land.

Rationale:
All 16 pinned 40-char commit SHAs in `.copilot-tracking/squad/members/issue-333/plans/ai-infra-pack-plan.md:107-108,153-155` and `ai-infra-pack-research.md:198-216` were verified via `gh api` and exist upstream. Byte-budget measurements in `ai-infra-pack-plan.md:119-123` match `Get-SquadContractByteCount` (`tests/tier0/SquadPackage.psm1:48-65`, `tests/tier0/HotFileBudget.Tests.ps1:94-126`): `squad-roster.instructions.md` has 146 B remaining (62,917 vs. 62,039+1,024 B), `squad-routing.instructions.md` 262 B, `squad-federation.instructions.md` 53 B, `squad.prompt.md` 52 B, `coordinator-init.md` 756 B (7,436/8,192 B), and Scribe `D7-5` cold core has 2 B (82,241/82,243 B in `tests/tier0/Packaging.Tests.ps1:192-203,309-316`). Agent body sizes in `P02-T03` have >11,900 chars of `PKG-04` headroom (`tests/tier0/SquadPackage.Tests.ps1:70-79`). Table-driven shared-resource deduplication in `Get-SquadPackResourceInventory` (`squad-src/.github/skills/squad/scripts/SquadCoordinator.psm1:192-263`) and `Invoke-Tier1LiveRun.ps1:150-244` supports sharing `azure-aigateway` across `foundry` and `ai-infra` while leaving non-adopting squads unchanged.

However, pressure-testing against git history (`#266` `e81ee051`, `#276` `fcc2e6c3`, `#282` `034f5191`) and Tier 0/docs gates exposed five implementation traps: (1) `ai-infra-pack-plan.md:350-355` fans out `D04` in parallel with `D02`, which breaks `P03-T01` verification (`Build-SquadPlugin.Tests.ps1:463`, `SquadCoordinator.psm1:198`); (2) `P03-T02` and `P05-T01` (`ai-infra-pack-plan.md:206,328`) invoke a non-existent `squad-src/.github/skills/squad/extensions/hve-squad-studio/studio.test.mjs`; (3) `P02-T02` misses `roster-catalog.md:27` and `profiles-and-packs.md:57` where `foundry` is still described as the sole active-pack gate; (4) `P04-T01` (`ai-infra-pack-plan.md:226`) warns only about `<pre><code>` parity, whereas `scripts/check-docs.mjs:52-63` also enforces identical DOM tag order, inline `<code>` order, and external link order between `docs/usage.html` and `docs/fr/usage.html`; and (5) `squad-roster.instructions.md` contains 5 normative sentences locked in `tests/tier0/baselines/prefill-baseline.json` (`Performance.Tests.ps1:382-450`) plus `RQ-04` (`SquadPackage.Tests.ps1:174-184`) that compaction must not touch.

Model identity: `claude-opus-4-6` (or host-assigned active session model)
Internal tool-call count: 47

Council role: challenger lens (verdict Conditional).
