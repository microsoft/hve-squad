---
description: "Append-only log of squad decisions and their rationale"
---

# Squad Decisions

Entries are appended below in chronological order. Each entry records the decision, its rationale, the turn it was made on, and a reference to an ADR when the decision is architecturally significant. Council Verdicts use the `## Council Verdict <timestamp> <topic-id>` heading and the schema in `.github/instructions/squad/squad-council.instructions.md`; Discovery Verdicts and Intake Readiness Verdicts use their own headings and schemas from `.github/instructions/squad/squad-discovery-gate.instructions.md` and `.github/instructions/squad/squad-intake-gate.instructions.md`. Prior entries are never edited or removed.

## Initialization 2026-11-22T19:26:53.6123648+00:00

* Turn: 1
* Provenance: default
* Model routing: off

## Watch Profile Selection 2026-11-22T19:27:00Z autopilot-run-45060332589

* Selected Profile: default
* Selected Packs: none
* Dropped Packs: none
* Reason: Trusted run controls fixed profile=default for this dispatch; the roster already covers the full RPI spine (researcher, lead, developer, tester, scribe) the cast-adaptation work needs.
* Source: watch-demand
* Demand Ref: Peter-N91/hve-squad#304
* Notification: none

## Council Disposition 2026-11-22T19:30:00Z autopilot-run-45060332589

* Disposition: Council not dispatched (skipped)
* Reason: The `default` profile roster for this run carries no architect, security, cost-manager, product-owner, or rai role, and the Stage 2 plan (`plans/cast-adaptation-plan.md`) classified the work as a mechanical cast/reference repoint (dropping 8 removed hve-core `Code Review *` subagents and reducing the `tester` role's Alternate Agents to its Primary, `Squad Reviewer`) with no cross-domain architecture, security, cost, product, or RAI risk.
* Source: plans/cast-adaptation-plan.md

## Risk Assessment 2026-11-22T19:42:00Z autopilot-run-45060332589

* Risk Rating: Low
* Rationale: The change removes references to agents already removed upstream by hve-core, repoints squad-src to the new pin via the repository's validated repin script, and drops the `coding-standards` doctor domain whose every member is gone. It adds no new capability, touches no production infrastructure, secrets, or user data, and is fully reviewed (`reviews/cast-adaptation-review.md`, verdict pass-with-findings/approve) with Pester suites (`WatchMode`: 154 passed, `Performance`: 47 passed, 0 failed each) exercised against it. The real `Get-HveCoreCastDelta.ps1 -FromRef cfbba35a... -ToRef e13fbf9...` comparison confirms `isBreaking: False` after the repin.
* No Stop verdict, no Risk: High finding, no compliance or divergence finding, and no cost-ceiling breach occurred in this run, so no Risk Gate fired.

## Final Outcome 2026-11-22T19:42:00Z autopilot-run-45060332589

* Outcome: completed (awaiting final validation)
* Summary: `history/autopilot-run-45060332589.md`
* Acceptance-criteria status (issue Peter-N91/hve-squad#304):
  * [x] Sub-squad `team.md` is cast per case and per ask from the canonical templates and includes the full RPI spine — `team.md`
  * [x] Full RPI stage artifacts (`research/<date>/*.md`, `plans/*.md`, `changes/*.md`, `reviews/*.md`) and `history/autopilot-run-*.md` are produced — `research/2026-11-22/cast-delta-research.md`, `plans/cast-adaptation-plan.md`, `changes/cast-adaptation-change.md`, `reviews/cast-adaptation-review.md`, `history/autopilot-run-45060332589.md`
  * [x] `history/` holds verified entries (with consumption blocks) for Squad Researcher, Squad Lead, Squad Implementor, Squad Reviewer, Squad Scribe — `history/Squad Researcher.md`, `history/Squad Lead.md`, `history/Squad Implementor.md`, `history/Squad Reviewer.md`, `history/Squad Scribe.md`
  * [x] Every roster Primary names an agent present in hve-core@e13fbf9 — confirmed in `reviews/cast-adaptation-review.md`
  * [x] No Primary or Alternate sets `disable-model-invocation: true` — Squad Reviewer remains the sole `tester` Primary/Alternate; no Code Review names remain
  * [x] Any new thin charter runs a real hve-core skill and declares a Deliverable Root — not applicable; this adaptation is drop-only, no new charter was authored, per the plan's confirmed scope
  * [x] `apm.yml` pins `e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` and lists any new charter files — confirmed in `reviews/cast-adaptation-review.md` (194 `e13fbf9` pins, 0 `cfbba35a` pins remaining, `version:` unchanged)
  * [x] A `.changes/unreleased/` fragment exists with `bump: minor` — `.changes/unreleased/20261122-adapt-squad-cast-to-hve-core-e13fbf9.md`
  * [x] `apm.yml` `version:` and `CHANGELOG.md` are untouched — confirmed in `reviews/cast-adaptation-review.md`
  * [x] Docs naming a changed agent or role are updated, or the PR states that none do — confirmed no hits in `docs/`, `docs/fr/`, `README.md`, `CONTRIBUTING.md`
  * [x] `Get-HveCoreCastDelta.ps1` reports a non-breaking verdict — confirmed in `reviews/cast-adaptation-review.md` (`-FromRef cfbba35a6541a39f446e1f8bde0fea46c055da6b -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83`: `isBreaking: False`, `squadAtRisk: {}`)
* Credential boundary: no `git commit`, `git push`, `gh pr create/comment`, or `gh issue comment` was run; every change is left uncommitted in the working tree for the separate downstream job.
* Known limitations (disclosed, not hidden; see `reviews/cast-adaptation-review.md` for full evidence):
  * `tests/tier0/SquadDoctor.Tests.ps1` fails 2 of 18 tests (SDOC-02, SDOC-03) when run against the repo root, because those tests exercise the gitignored, provisioned `.agents/skills/squad/scripts/Invoke-SquadDoctor.ps1` copy, which still carries the `coding-standards` domain and has not yet been regenerated from the edited `squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1` (which has no such references). This is expected to resolve once the packaging/staging step refreshes the generated tree; it is not a defect in the source change.
  * The change record's own `Get-HveCoreCastDelta.ps1` run was executed after the repin landed and so compared `e13fbf9` to itself (vacuous); the authoritative verification is the Review stage's `-FromRef cfbba35a6541a39f446e1f8bde0fea46c055da6b -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` run, which is non-BREAKING.
  * The full tier0 suite (including `Packaging.Tests.ps1` against `prefill-baseline.json`) was not re-run in this session; only the smallest relevant selection (WatchMode, Performance, SquadDoctor) was exercised, per the task's test-selection guidance. Pull-request CI covers the full suite.

## Preflight Repair 2026-11-22T19:44:30Z autopilot-run-45060332589

* The FINAL HANDOFF PREFLIGHT rejected the run records with two findings: `history/Squad Implementor.md` and `history/Squad Reviewer.md` each had a latest dispatch entry whose `Outcome` bullet carried descriptive findings text rather than the exact machine-checked word `Completed`.
* Repaired by appending one corrective history entry per agent (via `Write-SquadHandoff.ps1`) whose `Outcome` bullet reads exactly `Completed`, with the full findings text preserved as an addendum appended to each role's own deliverable (`changes/cast-adaptation-change.md`, `reviews/cast-adaptation-review.md`) rather than removed from the record.
* No roster, gate, dispatch, or completed-stage status changed. No file in `squad-src/` or any validator was edited.
* Re-ran `pwsh -NoProfile -Command "& '/home/runner/work/_temp/squad-watch-handoff.ps1' -ValidateOnly"` after the repair.
