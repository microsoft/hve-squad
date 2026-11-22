# Research: Adapt squad cast to hve-core e13fbf9 (issue #304)

Date: 2026-11-22 · Role: researcher · Precedent: `.copilot-tracking/squad/members/issue-248/` (research + `plans/cast-adaptation-plan.md`)

## Verified delta

`pwsh scripts/Get-HveCoreCastDelta.ps1 -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` (from cfbba35a): `isBreaking: True`. Removed outright (all were `Dispatchable=True`): Code Review Accessibility, Explainer, Functional, Orientation, Readiness, Security, Standards, Walkback (`.github/agents/coding-standards/subagents/code-review-*.agent.md`). Added: none. dispatchFlips, skills added/removed/flips, prompts added/removed: all empty. `Code Review` (user-invocable orchestrator, `coding-standards/code-review.agent.md`) is not in the removed list.

## Exact edit locations (from `grep -rn "Code Review" squad-src/`)

| File | Line(s) | Content / edit |
|---|---|---|
| `squad-src/.github/agents/squad/squad-coordinator.agent.md` | 49-55 | `agents:` frontmatter: Functional, Standards, Security, Accessibility, Readiness, Explainer, Walkback. Delete all 7 (Orientation is not listed). |
| `squad-src/.github/agents/squad/squad-federation-coordinator.agent.md` | 49-55 | Same 7 entries. Delete. |
| `squad-src/.github/skills/squad/references/roster-catalog.md` | 34 | `tester` row: Alternates (7 names, plus Orientation mentioned later in the line) and the Selection Cue are all Code Review based. Reduce to Primary `Squad Reviewer` only, with a cue like "implementation-versus-plan review → Squad Reviewer". Keep the "Reads, never writes tests" sentence. Check the full line for the Orientation mention. |
| same | 27 | Mentions `Code Review` (the user-invocable orchestrator) as a non-dispatchable example. This is still correct. Keep it. |
| same | 43 | `accessibility` row: "Distinct from the `tester` alternate `Code Review Accessibility`..." Reword to say diff-level accessibility review stays with `tester` / `Squad Reviewer`. |
| `squad-src/.github/instructions/squad/squad-roster.instructions.md` | 57 | `tester` row: Alternates `Code Review Functional, Code Review Standards` and their cue. Clear the Alternates cell and cue. |
| `squad-src/.github/instructions/squad/squad-routing.instructions.md` | 102 | "`Code Review Accessibility` per Selection Cue": reword to `Squad Reviewer`. |
| same | 193 | "Resolve `tester` to the matching review agent... `Code Review Functional`/`Security`": reword to the Primary `Squad Reviewer` only. |
| `squad-src/.github/skills/squad/references/gates-and-modes.md` | 80 | Same Selection Cue sentence. Reword to `Squad Reviewer`. |

### Hits the issue did not list (must also be handled)

| File | Line | Note |
|---|---|---|
| `squad-src/.github/skills/squad/references/seed-templates.md` | 41 | `tester` seed row with Functional/Standards Alternates and cues. |
| `squad-src/.github/skills/squad/references/federation.md` | 71 | Mentions Code Review. Inspect the line; likely a tester reference. |
| `squad-src/.github/skills/squad/squad-watch.workflow.yml` | 462, 495, 603 | Prompt text: "Squad Reviewer or cue-matched Code Review * Alternate". Reword to Squad Reviewer. Watch workflow tests may assert on this text, so rerun them. |
| `squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1` | 194 | `'coding-standards' = @('Code Review Functional', 'Code Review Standards', 'Code Review Security', 'Code Review Accessibility')` in the `$domains` table. Remove the whole `coding-standards` domain, because every member is gone. Otherwise the doctor will report missing agents. Check `$Scope` parameter validation and help text for the `coding-standards` value. |
| `tests/tier0/WatchMode.Tests.ps1` | 188 | Fixture roster row with `Code Review Functional` as an Alternate. Update the fixture. |
| `tests/tier0/SquadDoctor.Tests.ps1` | (case-insensitive hit) | Likely asserts on the `coding-standards` domain. Update with the doctor edit. |

Also check `tests/tier0/baselines/prefill-baseline.json` and `tests/squad-behavior-contract.md` (the #248 plan flagged both). Removing 7 `agents:` entries may shift the prefill baseline. Flag it rather than hand-edit it if a test fails.

## apm.yml

* `grep -c "Code Review" apm.yml` returns 0, because entries are paths with hyphens (`code-review-*.agent.md`).
* `apm.yml` lines 21-28 hold the 8 removed `subagents/code-review-*.agent.md#cfbba35a...` entries. Line 20, `coding-standards/code-review.agent.md`, stays (orchestrator still exists; confirm after the regeneration).
* Every hve-core entry is pinned `#cfbba35a6541a39f446e1f8bde0fea46c055da6b`.
* **Do not hand-edit.** Regenerate with `pwsh scripts/Update-ApmDependencies.ps1 -Ref e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83`. `-Ref` defaults to the SHA already pinned, so it must be passed explicitly. The script resolves the SHA, clones hve-core, rewrites only `dependencies.apm` (hve-core, then external cast, then squad entries) and pins squad self-references to `-SquadRef` if given. Do not touch `version:`. Afterwards, check that the 8 entries are gone and that every remaining hve-core pin is the new SHA.

## Dispatchability and Deliverable Roots (squad-roster.instructions.md)

* Dispatchability (line 102): an agent is dispatchable via `runSubagent`/`task` only if its frontmatter does not set `disable-model-invocation: true`. Never seed a non-dispatchable agent as Primary or Alternate. When the only fitting agent is user-invocable, the role is "thin charter needed": either author a squad-owned charter or escalate to the user. Worker agents that require a strict input contract (for example `RPI Researcher`) can never be role Primaries or Alternates.
* Deliverable Roots (line 137): `tester, challenger, intake-validator` → `.copilot-tracking/reviews/`. This is unchanged, and `Squad Reviewer` already writes there.
* No replacement exists. The delta reports no added agents, and the only remaining `coding-standards` agent is the `Code Review` orchestrator, which is user-invocable and not dispatchable. `Squad Reviewer` already runs the `code-review` skill (a skill is still shipped and listed in available skills) and covers the Primary role. `Accessibility Framework Assessor` and `Security Planner` / `Skill Assessor` cover product-level accessibility and security, so diff-level perspectives are not lost as roster capabilities. The Deferred Reviewer-Class Agents section already documents this pattern, and no deferral entry is needed for Code Review.

## Tooling

* Repin: see above. Verify: `pwsh scripts/Get-HveCoreCastDelta.ps1 -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` should report non-BREAKING (`squadAtRisk` empty).
* Fragment: `pwsh scripts/New-ChangeFragment.ps1 -Type Changed -Bump <patch|minor> -Title 'adapt squad cast to hve-core e13fbf9' -Body '- ...'`. Fragments go under `.changes/unreleased/` (existing example: `20261119-sync-hve-core-cfbba35.md`). Never edit `CHANGELOG.md` or `apm.yml` `version:`. The #248 precedent used `minor`. By the script's own rule, a removal under an existing idea fits `patch`, so the developer should decide. Bullets should list the repin, the 8 removed agents, and the `tester` role reduced to the Primary.

## Documentation

`grep` of `docs/`, `README.md`, and `CONTRIBUTING.md` for the 8 names (case-insensitive, hyphen or space forms) returned **no hits**. No doc edits are required. The developer should still confirm with a final sweep, including `docs/fr/`.

## Recommendation (confirmed)

This is a **drop-only adaptation**, matching the issue-248 template (edit squad-src → regenerate apm.yml via script → fragment → docs sweep → cast-delta verification → scribe):

1. Remove the 7 names from both coordinators' `agents:` lists (lines 49-55).
2. Drop all `tester` Alternates and Code Review Selection Cues. The Primary `Squad Reviewer` remains. Edit roster-catalog.md (34, 43), squad-roster.instructions.md (57), squad-routing.instructions.md (102, 193), gates-and-modes.md (80), seed-templates.md (41), federation.md (71), and squad-watch.workflow.yml (462, 495, 603).
3. Remove the `coding-standards` domain from `Invoke-SquadDoctor.ps1` and update `SquadDoctor.Tests.ps1` and `WatchMode.Tests.ps1`.
4. Repin apm.yml with `Update-ApmDependencies.ps1 -Ref e13fbf9...`.
5. Add the change fragment.
6. Verify that the cast delta is non-BREAKING and that no live `Code Review (Accessibility|Explainer|Functional|Orientation|Readiness|Security|Standards|Walkback)` remains in squad-src. The orchestrator name `Code Review` in roster-catalog.md line 27 stays valid.

No new thin charter is needed. Council is not applicable (a mechanical cast repoint with no architecture, security, cost, product, or RAI risk).

## Gaps

* Full text of roster-catalog.md line 34 (tail) and federation.md line 71 was truncated in my output. The developer should read them in full before editing.
* The prefill baseline impact is unverified until the tier0 tests run.
* The patch-versus-minor choice is left to the planner.

Planning readiness: **ready**.
