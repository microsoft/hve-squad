---
title: External Cast re-gate review for issue 312
description: Stage 5 review of the 2026-11-23 re-gate of four drifted azure-diagnose rows at microsoft/microsoft-sql 3224561
author: Squad Reviewer
ms.date: 2026-11-23
ms.topic: reference
---

Verdict: Pass-With-Findings

## Scope Reviewed

* `squad-src/.github/skills/squad/references/roster-catalog.md` (uncommitted diff).
* `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md` (uncommitted diff, line 40).
* `tests/tier0/PackRegistry.Tests.ps1` (uncommitted diff, `External Cast backstop pins and split-mode renewal automation` block).
* `.changes/unreleased/20261123-re-gated-azure-diagnose-external-cast-rows.md` (untracked).
* Gating artifacts: research, plan, and change record under `.copilot-tracking/squad/members/issue-312/`.

`git status --short` lists exactly the three modified files and one untracked fragment. `git diff --stat -- CHANGELOG.md apm.yml` is empty. The old names still appear only in the roster history narrative and in the absence assertion in the test. The `.github/agents/` and `.agents/skills/` copies are untracked local build outputs, so they are out of this diff's scope.

## Findings

| Severity | Location | Description | Suggested fix |
|---|---|---|---|
| Medium | `tests/tier0/PackRegistry.Tests.ps1` (Describe block) | Pester is not installed here (`Import-Module Pester` was denied by the environment, and `Invoke-Pester` is not recognised). The test cannot be executed, so the pass/fail claim rests on manual trace only. | Run `Invoke-Pester -Path tests/tier0/PackRegistry.Tests.ps1` in an environment with Pester 5 before merge. |
| Low | `tests/tier0/PackRegistry.Tests.ps1` (`$expectedSql` line) | The line `'investigate-sql-memory-issues', 'generate-migration-prerequisite-plan', 'get-migration-assessment',` is over-long and out of order (`design-azure-sql-schema` and `entra-id-auth` sit alphabetically in the list, but `generate-*` and `get-*` do not). The order does not affect the sort-based comparison. | Reflow the list one name per logical group for readability. Cosmetic only. |
| Low | `tests/tier0/PackRegistry.Tests.ps1` (absence assertion) | The absence check only runs on rows already filtered by `microsoft/microsoft-sql`. It would not catch a stale old name reintroduced under a different origin. | Acceptable as written; the plan does not require broader coverage. |
| Low | `.changes/unreleased/` | The change record (Files edited) names `20261123-regate-azure-diagnose-external-cast.md`, but the file on disk is `20261123-re-gated-azure-diagnose-external-cast-rows.md`. The on-disk file is the one the real `New-ChangeFragment.ps1` produced, so the record is stale, not the fragment. No duplicate remains. | Correct the filename in the change record. |
| Low | Change record, Validation section | `Sync-ExternalCast.ps1 -DryRun` reported all 156 rows as `NeedsReview` because the offline run had no network access. That makes the dry run a parse check only, not evidence that the eight new pins verify. | Re-run the dry run with network access before merge. |
| Info | Change record, Pre-edit verification | The eight successor installs, the LICENSE diff, and the packaged-reference checks are recorded as done. I did not re-run them. They rely on the implementer's own report. | None required for approval. |

No blocking defect found in the content of the diff. Nothing was fixed in production files.

## Plan Deviations

* **Done differently (documented):** the change record says the fragment came from `New-ChangeFragment.ps1` after a workaround for broken core-module autoloading. The plan allows generation by this script. Not a defect.
* **Done differently (minor):** the plan's Stage 5 step runs `Sync-ExternalCast.ps1 -DryRun`. The change record shows it ran offline with all rows `NeedsReview`. This is a limitation of the dry run, not a plan deviation.
* **Planned-not-done:** none. Docs, `apm.yml`, and `CHANGELOG.md` were correctly left untouched, consistent with the plan.
* **Done-not-planned:** the charter gap note and the narrative line about the `LICENSE` diff. Both are consistent with the plan's per-row decisions and narrative requirement.

## Conditions Honored

Not applicable. The plan records that Stage 3 (Council) was skipped.

## Verification Evidence

### 1. Roster-catalog correctness: met

Parsed `roster-catalog.md` with a script that mirrors the test's column split (`cells[3]` name, `cells[5]` origin, `cells[6]` install, `cells[10]` date):

* 21 `microsoft/microsoft-sql` rows, which matches the expected count. The sorted name set equals the test's `$expectedSql` set.
* The four old names (`diagnose-slow-query`, `diagnose-blocking-and-deadlocks`, `diagnose-resource-pressure`, `diagnose-connection-errors`) are absent from registered rows.
* Eight re-gated rows each have origin `` `microsoft/microsoft-sql` (`vendor-official`), `plugins/microsoft-sql-admin/skills/<name>/SKILL.md` ``, install `` `apm install microsoft/microsoft-sql/plugins/microsoft-sql-admin/skills/<name>#3224561abf68c98c1ab47e27d2a06e30acdd330a --target copilot` ``, date `2026-11-23`, licence `MIT`.
* `read-execution-plan` is unchanged: pinned `eeb1c68`, dated `2026-11-16`, `plugins/microsoft-sql/...`.
* The diff replaces the four old rows with exactly eight new rows, in the plan's order.

### 2. Blocklist correctness: met

Diff lines added to the External Cast Blocklist table:

```
+| `authentication-diagnostics` | skill | Gate step 5 — overlaps the registered `entra-id-auth` skill on `developer` ...; also gate step 6, no demand. ... | 2026-11-23 |
+| `deadlock-analysis` | skill | Gate step 6 — targets SQL Server and Managed Instance, not Azure SQL Database; `deadlock-analysis-azuresql` is the registered match. | 2026-11-23 |
```

Both reasons match the research table (lines 52, 59, 72, 79, 84). Each cites a specific gate step, and the columns match the existing `Resource | Kind | Reason | Recorded On` header.

### 3. Narrative paragraph: met

The diff keeps the `eeb1c68` paragraph as history (`-` line 46 and `+` line 47 are the same text, with the sentence still intact). It appends a dated paragraph beginning "Re-gated 2026-11-23 at `3224561...`", which follows the file's existing re-gate style.

### 4. Charter edit correctness: met

`squad-azure-diagnose.agent.md` line 40 names the eight successors with scopes. Its sentence structure is intact, with the `entra-id-auth` clause and the closing "Keep every SQL check strictly read-only ... hand remediations to ..." guardrail unchanged. The `10928`/`10929`/`10936` and `MAXDOP` gap is stated. The MCP-preferred and `sqlcmd` fallback note is present. No legacy name remains in the charter.

### 5. Test edit correctness and pass/fail: not executed; manual trace passes

Pester is not available. `Import-Module Pester` was denied by the environment, and the permission was not retried. I traced each assertion against the actual file:

* `Should -HaveCount 21`: 21 rows parsed. Pass.
* Sorted-name equality with `$expectedSql`: equal. Pass.
* For the eight `$regatedSql` names, path, install, and `2026-11-23` assertions all match the parsed cells (`ok5`, `ok6`, date all true). The `Do not auto-install` match holds for all eight. Pass.
* For the 13 untouched rows, the old regexes match the `eeb1c68` and `2026-11-16` values. Pass.
* Absence of the four old names: `@($rows | Where-Object ...) | Should -BeNullOrEmpty` returns null for the current table. Pass.
* Blocklist: `authentication-diagnostics` present; `deadlock-analysis\`` present (the backtick anchor is correct, since `deadlock-analysis-azuresql` would not match it); `eeb1c68` and `sql-migration-advisor@fredgis` still present. Pass.

One caution: the regex `^\`microsoft/microsoft-sql\`` for the `$sqlRows` filter also matches the eight re-gated rows, so the count of 21 includes them. This was accounted for. The manual trace is not a substitute for a real Pester run. See Finding 1.

### 6. Scope discipline: met

`git status --short` shows only `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md`, `squad-src/.github/skills/squad/references/roster-catalog.md`, `tests/tier0/PackRegistry.Tests.ps1`, and the untracked fragment. `git diff --stat -- CHANGELOG.md apm.yml` is empty, and `apm.yml` has no `microsoft-sql` entries. `docs/usage.html` and `docs/fr/usage.html` have no matches for the old names or the SHAs.

### 7. Change fragment correctness: met

The fragment has frontmatter `bump: patch` and `type: Changed`. Its body accurately describes the replacement, the blocklist additions, the charter and test edits, and `Closes #312`. It is the only fragment matching this change, so no stray duplicate remains. The change record's cited filename is stale (Finding 4).

### 8. Acceptance criteria from issue #312

Issue text was not provided in my inputs, so I assessed the criteria from the plan and research. The issue's four numbered domain requirements, as the dispatch states them:

1. **Run Verification Gate against escalated rows at upstream HEAD: partially met.** The implementer ran the disposable APM installs for all eight successors and the root LICENSE diff, and checked packaged references and MCP mentions, at `3224561`. The research recorded its gate steps 1 to 8 from frontmatter and targeted grep, not full reads (the research caveat, lines 89). The plan also lists the disposable installs and full LICENSE diff as gaps before the change. I did not re-run them. Evidence exists in the change record, but it is self-reported.
2. **Update pinned SHA and Verified On together across roster-catalog.md, docs, apm.yml, and PackRegistry.Tests.ps1 for passing rows: met.** The eight passing rows carry `3224561` and `2026-11-23` in the roster catalog and the test. The docs and `apm.yml` have no matching rows, so there is nothing to update there. The plan records this decision.
3. **Move failing or archived rows to the blocklist: met.** No successor failed the gate, so no registered row moved. The two non-registered candidates (`authentication-diagnostics`, `deadlock-analysis`) were blocklisted with step-specific reasons. The research's step-5 and step-6 reasons support these entries.
4. **Add a `.changes/unreleased/` fragment and run `Sync-ExternalCast.ps1 -DryRun` plus Tier 0: partially met.** The fragment exists and is valid. `Sync-ExternalCast.ps1 -DryRun` ran offline and reported all rows `NeedsReview`, so it did not verify the new pins. Tier 0 (Pester) did not run in this environment.

Overall: requirements 2 and 3 are met, and 1 and 4 are partially met. The unmet piece is execution evidence (Pester, the networked dry run, and an independent gate re-run). It is not a content defect.

## Review Record

This file is the review record. Its path is `.copilot-tracking/squad/members/issue-312/reviews/external-cast-regate-review.md`.
