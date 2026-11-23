---
description: "Append-only log of squad decisions and their rationale"
---

# Squad Decisions

Entries are appended below in chronological order. Each entry records the decision, its rationale, the turn it was made on, and a reference to an ADR when the decision is architecturally significant. Council Verdicts use the `## Council Verdict <timestamp> <topic-id>` heading and the schema in `.github/instructions/squad/squad-council.instructions.md`; Discovery Verdicts and Intake Readiness Verdicts use their own headings and schemas from `.github/instructions/squad/squad-discovery-gate.instructions.md` and `.github/instructions/squad/squad-intake-gate.instructions.md`. Prior entries are never edited or removed.

## Initialization 2026-11-23T19:47:42.4383638+00:00

* Turn: 1
* Provenance: default
* Model routing: off

## Watch Profile Selection 2026-11-23T19:49:00Z autopilot-run-45243130867

* Selected Profile: default
* Selected Packs: none
* Dropped Packs: none
* Reason: Trusted run controls fixed profile=default for this dispatch; the roster already covers the full RPI spine (researcher, lead, developer, tester, scribe) the roster-catalog re-verification work needs, and no pack is installed on this runner.
* Source: watch-demand
* Demand Ref: Peter-N91/hve-squad#312
* Notification: none

## Council Disposition 2026-11-23T20:05:00Z autopilot-run-45243130867

* Disposition: Council not dispatched (skipped)
* Reason: The `default` profile roster for this run carries no architect, security, cost-manager, product-owner, or rai role, and the Stage 2 plan (`plans/external-cast-regate-plan.md`) classified the work as a mechanical External Cast catalog re-gate (repointing 4 `azure-diagnose` rows registered against `microsoft/microsoft-sql` to their upstream-renamed successor skills after a verified upstream `head_path_missing` hazard, plus two narrow blocklist additions) with no cross-domain architecture, security, cost, product, or RAI risk. This mirrors the Council Disposition precedent in `.copilot-tracking/squad/members/issue-304/decisions.md` for a structurally similar cast/reference repoint.
* Source: plans/external-cast-regate-plan.md

## Risk Assessment 2026-11-23T20:35:00Z autopilot-run-45243130867

* Risk Rating: Low
* Rationale: The change repoints 4 already-registered, already-verified `azure-diagnose` External Cast rows to their upstream-renamed successors after `microsoft/microsoft-sql` PR #90 retired the old skill names and consolidated diagnostics under a new `microsoft-sql-admin` plugin (`reviews/external-cast-regate-review.md`, verdict `Pass-With-Findings`). It adds no new role, no new capability class, and no production infrastructure, secret, or user-data access; every successor skill is read-only triage, MIT-licensed, self-contained (no hooks, no hard MCP dependency), and was individually fetched and verified against upstream HEAD `3224561abf68c98c1ab47e27d2a06e30acdd330a` before being registered (`changes/external-cast-regate-change.md`). The root `LICENSE` diff between the old pin `eeb1c68` and the new pin `3224561` is empty (no licensing regression). Two candidates that failed the Verification Gate (`authentication-diagnostics`, gate step 5 overlap with the registered `entra-id-auth` skill; `deadlock-analysis`, gate step 6 wrong product scope) were correctly moved to the External Cast Blocklist instead of being registered.
* No Stop verdict, no Risk: High finding, no compliance or divergence finding, and no cost-ceiling breach occurred in this run, so no Risk Gate fired.
* Known limitation carried into Final Outcome: the Tier 0 Pester suite (`tests/tier0/PackRegistry.Tests.ps1`) could not execute in this sandboxed run because Pester is not installed here; the Reviewer independently traced the edited test's assertions against the actual current `roster-catalog.md` table content (row count 21, exact names, exact pins/paths/dates, blocklist matches) and confirmed they would pass, but this manual trace is not a substitute for the real suite. Pull-request CI runs the full suite and is the authoritative gate for this test.

## Final Outcome 2026-11-23T20:40:00Z autopilot-run-45243130867

* Outcome: completed (awaiting final validation)
* Summary: `history/autopilot-run-45243130867.md`
* Acceptance-criteria status (issue Peter-N91/hve-squad#312, "Domain requirements"):
  * [x] Run the full eight-step Verification Gate against each escalated row at its upstream HEAD commit SHA — confirmed for all 4 escalated rows and their 8 candidate successors in `research/2026-11-23/external-cast-regate-research.md` and re-verified with live upstream fetches in `changes/external-cast-regate-change.md` (frontmatter, `disable-model-invocation`, `tools:`, delegated-worker contract, HVE Core/sibling collision, prose/MCP prerequisites, licence posture)
  * [x] For rows that pass at the new SHA, update their pinned SHA and `Verified On` together across `roster-catalog.md` — done for all 8 successor rows (pin `3224561abf68c98c1ab47e27d2a06e30acdd330a`, `Verified On` `2026-11-23`); confirmed no `apm.yml`, EN/FR docs (`docs/usage.html`, `docs/fr/usage.html`), or `tests/tier0/PackRegistry.Tests.ps1` entries existed for these specific rows beyond the test file itself, which was updated; `scripts/Update-ApmDependencies.ps1` is not applicable to External Cast rows per the plan's own confirmation
  * [x] For rows that fail a gate check or are archived/superseded, move them to External Cast Blocklist with the failing step and date — done for `authentication-diagnostics` (gate step 5) and `deadlock-analysis` (gate step 6), both dated 2026-11-23
  * [~] Add a `.changes/unreleased/` patch fragment and run `./scripts/Sync-ExternalCast.ps1 -DryRun` plus Tier 0 — fragment added (`.changes/unreleased/20261123-re-gated-azure-diagnose-external-cast-rows.md`, `bump: patch`, `type: Changed`), generated by the real `New-ChangeFragment.ps1` script; `Sync-ExternalCast.ps1 -DryRun` ran successfully (exit 0, 156 rows parsed) but reported every row `NeedsReview` because this sandboxed run has no live network/GitHub API access for the script's own re-verification pass, not due to a defect in this change; the Tier 0 Pester suite could not execute here (Pester not installed) and needs to run in CI — see Risk Assessment
* Stage artifacts and per-role history:
  * Research: `research/2026-11-23/external-cast-regate-research.md`, `history/Squad Researcher.md`
  * Plan: `plans/external-cast-regate-plan.md`, `history/Squad Lead.md`
  * Council: not applicable (see Council Disposition above)
  * Implement: `changes/external-cast-regate-change.md`, `history/Squad Implementor.md`
  * Review: `reviews/external-cast-regate-review.md`, `history/Squad Reviewer.md`
  * Scribe: `history/Squad Scribe.md` (checkpoint after every stage)
* Credential boundary: no `git commit`, `git push`, `gh pr create/comment`, or `gh issue comment` was run; every change is left uncommitted in the working tree for the separate downstream job.
* Known limitations (disclosed, not hidden; see `reviews/external-cast-regate-review.md` for full evidence):
  * The Tier 0 Pester suite (`tests/tier0/PackRegistry.Tests.ps1`) was not executed in this sandboxed environment (no Pester module installed); independent manual verification of its assertions against the actual edited `roster-catalog.md` passed, but pull-request CI is the authoritative run of this suite.
  * `Sync-ExternalCast.ps1 -DryRun`'s own re-verification pass reported every row `NeedsReview` due to no live network access in this sandbox, not because of a defect in the edited rows; the roster-catalog table parse itself succeeded (156 rows read correctly).
  * This session's PowerShell runtime had broken core-module autoloading and broken absolute-path resolution; every squad script invocation in this run was executed through session-local workarounds (module pre-import, a corrected working-drive, and `.NET`-based path-resolving shims for file cmdlets) rather than any change to the shipped scripts themselves. No shipped script, skill, or instruction file was altered to work around this.
