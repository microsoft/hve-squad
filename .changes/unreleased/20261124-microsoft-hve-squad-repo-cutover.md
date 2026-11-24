---
bump: patch
type: Changed
---

- **Package and plugin references still pointed to the pre-transfer repository.** Cut over self-reference paths in `apm.yml`, plugin and marketplace metadata in `scripts/Build-SquadPlugin.ps1`, release/sync/docs scripts, workflows, issue templates, shipped `squad-src` artifacts, and bilingual documentation from `Peter-N91/hve-squad` and `peter-n91.github.io/hve-squad` to `microsoft/hve-squad` and `microsoft.github.io/hve-squad`, and added `.github/CODEOWNERS` for `@microsoft/ocp-isv-admins`.
