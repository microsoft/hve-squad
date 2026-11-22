# Plan: Adapt squad cast to hve-core e13fbf9 (issue #304)

Date: 2026-11-22 · Role: lead · Research: `../research/2026-11-22/cast-delta-research.md` · Precedent: `../../issue-248/plans/cast-adaptation-plan.md`

## Decision

Drop-only adaptation. The 8 `Code Review *` subagents (Accessibility, Explainer, Functional, Orientation, Readiness, Security, Standards, Walkback) were removed upstream. The `tester` role is reduced to its Primary `Squad Reviewer`; all Code Review Alternates and their Selection Cues are removed. The `coding-standards` doctor domain is removed entirely. No new thin charter (matches issue-248). Release bump: **minor** (the shipped roster changes).

Council: **NOT applicable.** Mechanical cast/reference repoint; no architecture, security, cost, product, or RAI risk.

## Phases

1. **Edit squad-src, scripts, tests** (D1-D4). Developer.
2. **Repin and fragment** (D5-D6). Developer.
3. **Docs confirmation sweep** (D7). Developer.
4. **Review** (D8). Tester.
5. **Scribe**: history, decisions, state.

Success: no live `Code Review (Accessibility|Explainer|Functional|Orientation|Readiness|Security|Standards|Walkback)` in `squad-src/`; cast delta non-BREAKING; tier0 tests pass.

## File-by-file edits

| # | File | Edit |
|---|---|---|
| 1 | `squad-src/.github/agents/squad/squad-coordinator.agent.md` | Lines ~49-55: delete the 7 Code Review `agents:` entries (Orientation was never listed). |
| 2 | `squad-src/.github/agents/squad/squad-federation-coordinator.agent.md` | Same 7 deletions. |
| 3 | `squad-src/.github/skills/squad/references/roster-catalog.md` | Line 34: `tester` row → Primary `Squad Reviewer`, no Alternates, cue like "implementation-versus-plan review → Squad Reviewer"; keep "Reads, never writes tests"; read the full line (incl. Orientation mention) first. Line 43: reword `accessibility` row so diff-level accessibility review stays with `tester` / `Squad Reviewer`. Line 27 (`Code Review` orchestrator as non-dispatchable example) stays. |
| 4 | `squad-src/.github/instructions/squad/squad-roster.instructions.md` | Line 57: clear `tester` Alternates cell and cue. |
| 5 | `squad-src/.github/instructions/squad/squad-routing.instructions.md` | Lines 102, 193: resolve to `Squad Reviewer` only. |
| 6 | `squad-src/.github/skills/squad/references/gates-and-modes.md` | Line 80: same reword. |
| 7 | `squad-src/.github/skills/squad/references/seed-templates.md` | Line 41: remove Alternates/cues from seeded `tester` row. |
| 8 | `squad-src/.github/skills/squad/references/federation.md` | Line 71: read in full; update consistent with the drop. |
| 9 | `squad-src/.github/skills/squad/squad-watch.workflow.yml` | Lines 462, 495, 603: "Squad Reviewer or cue-matched Code Review * Alternate" → "Squad Reviewer". |
| 10 | `squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1` | Line ~194: remove the whole `coding-standards` entry from `$domains`; update `$Scope` ValidateSet/help/comments naming it. |
| 11 | `tests/tier0/WatchMode.Tests.ps1` | Line ~188 fixture: drop the Code Review Functional Alternate. |
| 12 | `tests/tier0/SquadDoctor.Tests.ps1` | Remove/adjust `coding-standards` domain assertions and scope cases. |
| 13 | `tests/tier0/baselines/prefill-baseline.json`, `tests/squad-behavior-contract.md` | Check for stale assertions on removed agents or domain. Follow issue-248 approach (`Performance.Tests.ps1` and contract edits): update or add retirement allowances. If prefill shifts, regenerate via the repo's documented baseline mechanism; do not guess-edit. |
| 14 | `apm.yml` (script only) | `pwsh scripts/Update-ApmDependencies.ps1 -Ref e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83`. Never hand-edit; leave `version:`. Verify the 8 `subagents/code-review-*` entries vanish and `coding-standards/code-review.agent.md` remains. |
| 15 | `.changes/unreleased/` | `pwsh scripts/New-ChangeFragment.ps1 -Type Changed -Bump minor -Title 'adapt squad cast to hve-core e13fbf9' -Body '- Repin to hve-core e13fbf9\n- Removed the 8 Code Review subagents from the cast\n- tester role reduced to its Primary Squad Reviewer; coding-standards doctor domain removed'`. Never edit `CHANGELOG.md`. |
| 16 | `docs/` (incl. `docs/fr/`), `README.md`, `CONTRIBUTING.md` | Final case-insensitive sweep (space and hyphen forms). Research found no hits; record "none apply" or fix EN+FR together. |

## Deliverables (execution order)

| # | Deliverable | Owner | Artifact | Depends on | Batch |
|---|---|---|---|---|---|
| D1 | Coordinator `agents:` removals (edits 1-2) | developer (Squad Implementor) | 2 agent files | none | A |
| D2 | Roster/routing/reference/watch reword (edits 3-9) | developer | 7 squad-src files | none | A |
| D3 | Doctor domain removal (edit 10) | developer | `Invoke-SquadDoctor.ps1` | none | A |
| D4 | Test/baseline/contract updates (edits 11-13) | developer | tier0 tests, baseline, contract | D2, D3 | B |
| D5 | apm.yml repin (edit 14) | developer | regenerated `apm.yml` | D1-D3 | B |
| D6 | Change fragment (edit 15) | developer | `.changes/unreleased/*.md` | D5 | C |
| D7 | Docs confirmation sweep (edit 16) | developer | none-apply note or doc edits | none | A |
| D8 | Verification: `pwsh scripts/Get-HveCoreCastDelta.ps1 -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` non-BREAKING (`squadAtRisk` empty); `grep -rnE "Code Review (Accessibility\|Explainer\|Functional\|Orientation\|Readiness\|Security\|Standards\|Walkback)" squad-src/` empty; tier0 tests (WatchMode, SquadDoctor, Performance, prefill) pass; `git diff apm.yml` shows `version:` untouched | tester (Squad Reviewer) | review under `.copilot-tracking/reviews/` | D1-D7 | D |
| D9 | State/history/decisions entry | scribe | `issue-304/{history,decisions.md,state.json}` | D8 | E |

## Implement Shape

`single-build`: one artifact-owning role (developer); batch A items are independent file groups done in one pass. Tester follows.

## Assumptions and Open Decisions

* ASM-1: Removing Alternates is a capability reduction; `minor` accepted as instructed.
* ASM-2: Prefill baseline may shift; regenerate through the documented mechanism rather than hand-edit.
* ASM-3: `Squad Reviewer` (via the `code-review` skill) fully covers tester; no replacement charter. Deferred: any future charter wrapping the `Code Review` orchestrator.
* Open: none.

## Gaps in Research

* Full tails of roster-catalog.md line 34 and federation.md line 71 not captured; developer reads them before editing.
* Prefill baseline impact unverified until tier0 runs.
