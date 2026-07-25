---
"vibedev-ecosystem": minor
---

Add 3rd satellite skill `/vd-arch-review` to the VibeDev family. User-invoked, dev-only. 3 sub-commands mapping to 3 levels of depth (macro/meso/micro) inspired by Sandeco Macedo's 5-level verification ladder (arXiv:2607.00038) and mattpocock/skills `codebase-design` vocabulary. Detects layer separation, anti-patterns, abstraction leaks, coupling, dep vs custom, test coverage. State persists in `ARCH_MAP.md` + `.arch-review/`. No behavior break to existing 11 commands.
