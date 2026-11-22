# Change record: issue #304 cast adaptation to hve-core e13fbf9

## Files modified
- squad-src agents: squad-coordinator, squad-federation-coordinator (removed 7 Code Review `agents:` entries)
- squad-src references: roster-catalog.md (tester row -> Squad Reviewer only; accessibility cross-ref reworded), gates-and-modes.md, seed-templates.md, federation.md
- squad-src instructions: squad-roster, squad-routing
- squad-src skills/squad: squad-watch.workflow.yml (3 prompt rewords), scripts/Invoke-SquadDoctor.ps1 (coding-standards domain + ValidateSet entry removed)
- tests/tier0/WatchMode.Tests.ps1 (fixture Alternate cleared), tests/tier0/SquadDoctor.Tests.ps1 (dropped code-review-functional from inventory fixture)
- apm.yml (via Update-ApmDependencies.ps1 only; 8 code-review subagent entries gone; version untouched; coding-standards/code-review agent+skill remain)
- .changes/unreleased/20261122-adapt-squad-cast-to-hve-core-e13fbf9.md (new, Changed/minor)

## Commands and results
- Update-ApmDependencies.ps1 -Ref e13fbf9...: OK
- New-ChangeFragment.ps1: created
- Get-HveCoreCastDelta.ps1 -ToRef e13fbf9...: non-breaking, squadAtRisk empty (run after repin)
- grep for 8 removed names in squad-src/: no results
- Pester WatchMode + Performance (SourceRoot=repo root): pass; 201 passed, 0 failed
- D7 docs/README/CONTRIBUTING sweep: no hits; none apply. tests baseline/behavior-contract: no references to removed agents.

## Follow-ups for Review
- SquadDoctor.Tests (PackageRoot=repo root): 16 pass, 2 fail (SDOC-02, SDOC-03) because the test runs the generated, not-yet-regenerated `.agents/skills/squad/scripts` Doctor that still has coding-standards; rerun after regeneration. Verify generated sync.

## Outcome Addendum

Final machine-checked status: Completed. The SquadDoctor (SDOC-02/SDOC-03) follow-up above is a disclosed limitation tied to the gitignored, provisioned `.agents/` generated tree, not a defect in this stage's deliverables; see `reviews/cast-adaptation-review.md` for the confirming diff.
