# VibeDev is not for monorepos

VibeDev assumes a single project per `PROJECT_STATE.md`. Multi-package workspaces (monorepos) need different governance patterns.

## What VibeDev assumes
- 1 `PROJECT_STATE.md` per project root
- 1 set of phases (1-8) applied to the project
- 1 stack, 1 cost estimate, 1 deploy target

## What VibeDev does NOT handle
- Multiple `package.json` / `pyproject.toml` in one repo
- Sub-package version coordination
- Inter-package dependency governance
- Selective deployment of one package from the monorepo

## For monorepos, use
- **Turborepo / Nx / Lerna** for orchestration
- **Changesets** for versioning
- **Per-package READMEs** with their own state

You can run VibeDev **per package** in a monorepo (one `PROJECT_STATE.md` per package), but the cross-package coordination is not in scope.

## Why this is out of scope

Monorepo governance is a different problem space with different tools, different community, and different best practices. Mixing the two would dilute VibeDev's clarity.
