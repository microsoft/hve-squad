---
description: "Autopilot-run summary for topic issue-304"
---

# Autopilot Run: issue-304

* Topic: Adapt squad cast to hve-core@e13fbf9 (Peter-N91/hve-squad#304) — drop the 8 removed `Code Review *` subagents from squad-src, reduce the `tester` role to its Primary `Squad Reviewer`, remove the dangling `coding-standards` doctor domain, repin `apm.yml`, and add a release fragment.
* Outcome: completed (awaiting final validation)
* Trigger Source: workflow_dispatch
* Trigger Ref: Peter-N91/hve-squad#304
* Trigger Event ID: workflow_dispatch:45060332589
* Trigger Actor: Peter-N91
* Trigger Received At: 2026-11-22T19:22:03.169Z

## Stages

| Stage     | Role(s)    | Dispatch Record                 | Result   | Gate Fired |
| --------- | ---------- | -------------------------------- | -------- | ---------- |
| research  | researcher | `history/Squad Researcher.md`    | complete | none       |
| plan      | lead       | `history/Squad Lead.md`          | complete | none       |
| council   | none       | n/a                               | skipped — this roster carries no architect, security, cost-manager, product-owner, or rai role, and the change is a mechanical cast/reference repoint with no cross-domain architecture, security, cost, product, or RAI risk | none |
| implement | developer  | `history/Squad Implementor.md`   | complete | none       |
| review    | tester     | `history/Squad Reviewer.md`      | complete | none       |

## Notes

* Deliverables: research `research/2026-11-22/cast-delta-research.md`; plan `plans/cast-adaptation-plan.md`; change record `changes/cast-adaptation-change.md`; review `reviews/cast-adaptation-review.md`.
* Working-tree deliverables outside `.copilot-tracking/`: `apm.yml` (repinned to `e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` via `scripts/Update-ApmDependencies.ps1`, `version:` unchanged), one fragment at `.changes/unreleased/20261122-adapt-squad-cast-to-hve-core-e13fbf9.md` (bump: minor), and edits under `squad-src/` (`.github/agents/squad/squad-coordinator.agent.md`, `.github/agents/squad/squad-federation-coordinator.agent.md`, `.github/instructions/squad/squad-roster.instructions.md`, `.github/instructions/squad/squad-routing.instructions.md`, `.github/skills/squad/references/federation.md`, `.github/skills/squad/references/gates-and-modes.md`, `.github/skills/squad/references/roster-catalog.md`, `.github/skills/squad/references/seed-templates.md`, `.github/skills/squad/scripts/Invoke-SquadDoctor.ps1`, `.github/skills/squad/squad-watch.workflow.yml`), plus `tests/tier0/SquadDoctor.Tests.ps1` and `tests/tier0/WatchMode.Tests.ps1` fixture updates.
* Acceptance-criteria disposition (issue #304): all 11 criteria are met — see `decisions.md#final-outcome-2026-11-22t194200z-autopilot-run-45060332589` for the itemized evidence, including the real `Get-HveCoreCastDelta.ps1 -FromRef cfbba35a6541a39f446e1f8bde0fea46c055da6b -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` non-BREAKING verdict.
* No Impactful-Action was taken: no `git commit`, `git push`, merge, force-push, deploy, migration, deletion, or secret rotation. All changes are left uncommitted in the working tree per the Watch Mode credential boundary; a separate automated step assembles the pull request from this working tree.
* This run surfaced no Risk Gate finding (no Stop verdict, no Risk: High, no compliance or divergence finding, no cost-ceiling breach), so `members/issue-304/decisions.md` carries a Risk Assessment heading rather than a Risk Gate heading.
* Known limitations (disclosed in `decisions.md`'s Final Outcome entry): two `SquadDoctor.Tests.ps1` tests (SDOC-02, SDOC-03) fail against the stale, gitignored, provisioned `.agents/skills/squad/scripts/Invoke-SquadDoctor.ps1` copy rather than the edited `squad-src` source; the change record's own cast-delta run was executed post-repin and is vacuous (the authoritative delta is the Review stage's `-FromRef`/`-ToRef` run, which is non-BREAKING); and the full tier0 suite (including `Packaging.Tests.ps1`) was not re-run in this session, deferred to pull-request CI per the task's smallest-relevant-selection guidance.
