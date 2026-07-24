# VibeShield does not replace VibeDev

VibeShield is a **satellite skill** that VibeDev invokes via handoff. It does not absorb VibeDev's responsibilities.

## What VibeShield does
- Audits code in the active sub-task
- Returns verdicts (OK / REVISAR / BLOQUEAR)
- Writes audit results to the VibeDev-managed `PROJECT_STATE.md`
- Translates verdicts to VibeDev's phase language

## What VibeShield does NOT do
- Decide which phase the project is in
- Plan the project (that's VibeDev's `/vd-plan`)
- Build code (VibeDev's `/vd-build`)
- Validate completion (VibeDev's `/vd-check`)
- Manage the project lifecycle

## The architecture

```
VibeDev (orchestrator)
   │
   ├── /vd-spark   → discovery
   ├── /vd-start   → bootstrap
   ├── /vd-status  → state
   ├── /vd-plan    → planning
   ├── /vd-build   → execution
   ├── /vd-check   → validation
   ├── /vd-launch  → communication
   └── /vd-kill    → termination
            │
            ├── triggers ──→ VibeShield (audit on G1-G7)
            │                  │
            │                  ├── C1-C8 categories
            │                  ├── OK/REVISAR/BLOQUEAR
            │                  └── writes back to state
            │
            └── gets verdict back to continue /vd-build

VibeShield never runs without VibeDev orchestration. VibeDev never
audits without VibeShield triggering.
```

## Why this is out of scope

The handoff architecture (`vibedev/references/handoffs.md` ↔ `vibeshield/references/handoff-vibedev.md`) is the **design**. It's been working since v1.0.0 of both skills.

If a feature seems to require VibeShield to do project management, it actually requires VibeDev changes. If it seems to require VibeDev to do security audit, it actually requires VibeShield to be triggered.
