---
bump: patch
type: Changed
---

- **Re-gated drifted azure-diagnose External Cast rows.** Replaced the four `microsoft/microsoft-sql` rows that upstream removed (`diagnose-slow-query`, `diagnose-blocking-and-deadlocks`, `diagnose-resource-pressure`, `diagnose-connection-errors`) with eight successor skills from `plugins/microsoft-sql-admin`, re-gated at `3224561` and verified on 2026-11-23. Blocklisted `authentication-diagnostics` and `deadlock-analysis`, and updated the Squad Azure Diagnose charter and the Tier 0 registry test (`squad-src/.github/skills/squad/references/roster-catalog.md`, `squad-src/.github/agents/squad/squad-azure-diagnose.agent.md`, `tests/tier0/PackRegistry.Tests.ps1`). Closes #312.
