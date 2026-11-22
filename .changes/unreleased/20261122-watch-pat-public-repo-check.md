---
bump: patch
type: Fixed
---

- **Watch Mode rejected zero-repository-scope Copilot PATs owned by repository maintainers on public repositories.** `Verify COPILOT_GITHUB_TOKEN least privilege` now probes fine-grained token repository and push scope directly when `GET /repos/{owner}/{repo}` reports the owner's user-level permissions on HTTP 200, while still failing closed on classic and repository-write tokens (`.github/workflows/squad-watch.yml`, `squad-src/.github/skills/squad/squad-watch.workflow.yml`).
