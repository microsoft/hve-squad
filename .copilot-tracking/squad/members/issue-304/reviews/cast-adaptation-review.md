# Review: Adapt squad cast to hve-core@e13fbf9 (issue #304)

Reviewer: Squad Reviewer (tester) · Date: 2026-11-22
Inputs: plan `plans/cast-adaptation-plan.md`, research `research/2026-11-22/cast-delta-research.md`, change record `changes/cast-adaptation-change.md`.

Verdict: Pass-With-Findings

Scope: D1-D9 per plan. D9 (scribe state) is out of reviewer scope and not assessed here.

## Findings

| # | Severity | Location | Finding |
|---|---|---|---|
| F-1 | Medium (non-blocking for source; blocks the SDOC tier0 run until regenerated) | `.agents/skills/squad/scripts/Invoke-SquadDoctor.ps1` (gitignored, `.gitignore:17`) | Stale generated copy still has the `coding-standards` ValidateSet value (line 22) and domain entry (line 194). The source at `squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1` has neither. The two SquadDoctor failures come from this stale copy, not from squad-src. The change record says the tree is regenerated before staging/CI. I did not verify that regeneration step. Confirm it in staging before merge. |
| F-2 | Low (evidence gap) | Change record, "Commands and results" | The recorded `Get-HveCoreCastDelta.ps1 -ToRef e13fbf9...` run was made after the repin, so it compared e13fbf9 to e13fbf9 and was vacuous. The output showed `hasDelta: False`. The real check is `-FromRef cfbba35a... -ToRef e13fbf9...`, which I ran (see D8). Note the correction in the change record. |
| F-3 | Low (unverified) | `tests/tier0/baselines/prefill-baseline.json` | The plan (ASM-2) expected the prefill baseline to shift. I found no `code-review`/`coding-standards` references in it and no edits to it. I did not run `Packaging.Tests.ps1`, which consumes this baseline. Confirm it passes in the full tier0 run. |
| F-4 | Info | `squad-src/.github/skills/squad/references/roster-catalog.md:27` | Still names `Code Review` as a non-dispatchable user-invocable orchestrator. This is correct and intended. |
| F-5 | Info | `tests/tier0/SquadDoctor.Tests.ps1` diff | The only change is dropping `hve-core:code-review-functional` from the inventory fixture. No `coding-standards` assertion existed, so the plan's "remove/adjust" step had nothing else to change. |

No High or Critical findings.

## Verification evidence

### D1: coordinator `agents:` lists

`grep -nE "Code Review" squad-src/.github/agents/squad/*.agent.md` returned no output. Both `squad-coordinator.agent.md` and `squad-federation-coordinator.agent.md` have the 7 entries removed (diff hunk `@@ -46,13 +46,6 @@`). **Pass.**

### D2: tester and reference repoints

- `roster-catalog.md:34`: `tester | Squad Reviewer | (no Alternates) | Implementation-versus-plan review of a diff → Squad Reviewer.` Only the Primary remains. **Pass.**
- `roster-catalog.md:43`: now reads "Diff-level accessibility review stays with `tester` / `Squad Reviewer`." **Pass.**
- `federation.md:71`: names `tester (Squad Reviewer)`. **Pass.**
- `squad-watch.workflow.yml:462, 466, 495, 603`: all say `Squad Reviewer`. **Pass.**
- `squad-roster.instructions.md`, `squad-routing.instructions.md`, `gates-and-modes.md`, `seed-templates.md` are modified in the diff. I confirmed `grep "Code Review"` finds no Alternate references. **Pass.**
- Remaining `Code Review` hits in squad-src: only `roster-catalog.md:27`, which is intended (F-4).

### D3: doctor domain

`squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1`: `grep -niE "coding-standards|coding standards"` returned no output. The ValidateSet at line 22 has no `coding-standards`. The `$domains` table at lines 191-193 has no `coding-standards` key. **Pass.**

### D4: tests and baselines

- `WatchMode.Tests.ps1:188`: tester fixture row is `| tester | | Squad Reviewer | | runSubagent / task | fast | ...`. **Pass.**
- `SquadDoctor.Tests.ps1`: the inventory fixture no longer has `code-review-functional`. **Pass.**
- `tests/squad-behavior-contract.md`: `grep` for Code Review, coding-standards, and removed-agent names matches only `tester` lines (RTE-04/05, RQ-*). None of them reference a removed agent. **Pass.**
- `prefill-baseline.json`: no stale references and not modified (F-3). **Pass with note.**

### D5: apm.yml

- `git diff --stat apm.yml`: 194 insertions, 202 deletions. The large diff is the SHA change on every hve-core pin.
- Pins: `grep -c cfbba35a apm.yml` returns 0. `grep -c e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83 apm.yml` returns 194.
- Non-SHA diff (SHA stripped, sorted, compared to HEAD): the only difference is 8 removed lines, all `coding-standards/subagents/code-review-{accessibility,explainer,functional,orientation,readiness,security,standards,walkback}.agent.md`.
- `coding-standards/code-review.agent.md` (line 20) and `coding-standards/code-review` skill (line 113) remain.
- `version:` is `0.21.0` at HEAD and `version: 0.21.0` in the working tree (line 4). Untouched. **Pass.**

### D6: change fragment and CHANGELOG

- `.changes/unreleased/20261122-adapt-squad-cast-to-hve-core-e13fbf9.md`: `bump: minor`, `type: Changed`, body lists the repin, the 8 removed agents, the tester reduction, and the doctor domain removal. Matches the plan.
- `git status --short CHANGELOG.md`: empty, so untouched. **Pass.**

### D7: docs sweep

`grep -rliE "code.review (accessibility|explainer|functional|orientation|readiness|security|standards|walkback)" docs README.md CONTRIBUTING.md` returned no files. This covers `docs/fr/`. **Pass.**

### D8: verification

1. Cast delta, repin-state check (vacuous, see F-2):
   `pwsh scripts/Get-HveCoreCastDelta.ps1 -ToRef e13fbf9...` → `From: e13fbf9...`, `hasDelta: False`, `isBreaking: False`.
2. Cast delta, real comparison:
   `pwsh scripts/Get-HveCoreCastDelta.ps1 -FromRef cfbba35a6541a39f446e1f8bde0fea46c055da6b -ToRef e13fbf9cdd3f93eacdfbb3c3ea0631c3cbe79e83` → `hasDelta: True`, `isBreaking: False`, `removed` lists the 8 Code Review agents (all `Dispatchable=True`), `added: {}`, `dispatchFlips: {}`, `skills*: {}`, `prompts*: {}`, `squadAtRisk: {}`. **Non-BREAKING, as expected.**
3. Grep: `grep -rnE "Code Review (Accessibility|Explainer|Functional|Orientation|Readiness|Security|Standards|Walkback)" squad-src/` → no output, exit 1. **Pass.**
4. Pester (tier0, required parameters supplied via `New-PesterContainer -Data`; the command `Invoke-Pester -Data` is not supported in Pester 5.9):
   - WatchMode (`SourceRoot`=repo root): **Passed 154, Failed 0, Skipped 0.** Pass.
   - Performance (`SourceRoot`=repo root): **Passed 47, Failed 0, Skipped 0.** Pass.
   - SquadDoctor (`PackageRoot`=repo root): **Passed 16, Failed 2, Skipped 0.** Failing: "checks all seven host domains without initializing or writing files" and "returns repaired then healthy against a real initialized squad and ledger". Both expected 0 and got 2. The JSON shows `domain:coding-standards` `fail` with "Missing host agent", which the stale `.agents` tree produces (F-1). Under the SDOC-02/03 ids the change record names, this is the disclosed failure. Not a squad-src defect.

### Disclosed-limitation check (generated vs source doctor)

`diff squad-src/.github/skills/squad/scripts/Invoke-SquadDoctor.ps1 .agents/skills/squad/scripts/Invoke-SquadDoctor.ps1` shows exactly two differences:
- Line 22: the `.agents` copy has `, 'coding-standards'` in ValidateSet.
- After line 193: the `.agents` copy has `'coding-standards' = @('Code Review Functional', ...)`.

Both differences are the removed `coding-standards` domain. The source has neither. The SDOC-02/03 failures are caused by the generated copy, which matches the Implementor's disclosure.

### D9: out of scope

Scribe state (history, decisions, state.json) is not reviewed here. Note: `.copilot-tracking/squad/history/issue-304.md` exists in the working tree.

## Plan deviations

- Planned-not-done: none found in D1-D8.
- Done-differently: the cast-delta verification in the change record was run against the already-repinned SHA and is vacuous (F-2). The real delta was verified separately (D8.2).
- Done-not-planned: `tests/tier0/SquadDoctor.Tests.ps1` changed only the fixture (F-5). The plan expected assertion changes, but none were needed.

## Conditions honored

Not applicable. The plan records council as not applicable.

## Conclusion

The source-side changes (D1-D7) match the plan. The real cfbba35→e13fbf9 delta is non-BREAKING, and no live removed-agent references remain in squad-src. The two SquadDoctor failures come from the gitignored generated `.agents` tree, not from source. Before merge, confirm (a) the packaging/staging step regenerates `.agents/` from squad-src (F-1), (b) the full tier0 run including `Packaging.Tests.ps1` passes against the prefill baseline (F-3), and (c) the change record notes the vacuous delta run (F-2).

## Outcome Addendum

Final machine-checked status: Completed. Verdict: pass-with-findings (approve); full findings and evidence are recorded above and carried into `decisions.md`'s Final Outcome entry.
