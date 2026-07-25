# `/vd-arch-review` is not an architect replacement

`/vd-arch-review` is a **static analysis skill** that flags architectural anti-patterns and debts. It does not replace an architect or make architectural decisions.

## What `/vd-arch-review` does
- Reads the codebase structure (no execution)
- Identifies anti-patterns: layers leak, god objects, generic filenames, dep vs custom
- Maps modules, bounded contexts, data flow
- Outputs a list of findings with severity and effort estimate
- Persists results in `ARCH_MAP.md` and `.arch-review/`

## What `/vd-arch-review` does NOT do
- **Refactor code** — only points to what to refactor
- **Decide if modernization is worth it** — that's a human decision
- **Run tests or build** — static analysis only
- **Replace an architect** — humans still make architectural decisions
- **Detect runtime race conditions** — that requires execution context
- **Do penetration testing** — that's security, not architecture
- **Enforce a specific style** — finds issues, doesn't dictate solution

## For those things, use
- **A human architect** for architectural decisions
- **VibeShield `code-review`** for per-PR standards + spec review
- **VibeShield `audit`** for security C1-C8 categories
- **mattpocock/skills `codebase-design`** for shared vocabulary (deep modules, seams, adapters)
- **A refactoring session** for actually fixing the debts `/vd-arch-review` finds

## Why this is out of scope

Architectural decisions involve trade-offs that depend on:
- Team capacity
- Business priorities
- Future roadmap
- User feedback
- Risk tolerance

A static analysis skill can flag patterns but cannot weigh these factors. `/vd-arch-review` is a **diagnostic tool**, not a decision-maker.

## Relationship to other VibeDev family skills

| Skill | Focus | Trigger | Verdict |
|---|---|---|---|
| **VibeDev** (`/vd-*`) | Project governance (phases, state) | User-invoked | Phase gates |
| **VibeShield** (audit) | Security C1-C8 | Auto via G1-G7 | OK/REVISAR/BLOQUEAR |
| **VibeShield** (`code-review`) | Per-PR Standards + Spec | User-invoked | OK/REVISAR/BLOQUEAR |
| **`/vd-arch-review`** | Architecture, layers, debt | User-invoked | OK/REVISAR/BLOQUEAR |

All three are complementary. None replaces the others.
