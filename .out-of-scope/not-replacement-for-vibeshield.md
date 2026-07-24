# VibeDev is not a replacement for VibeShield

VibeShield is a separate skill in the same ecosystem. VibeDev does not absorb its functionality.

## What VibeDev does
- Manages project lifecycle
- Validates user-facing features (`/vd-check`)
- Triggers VibeShield when security-sensitive code is touched

## What VibeDev does NOT do
- Audit authentication code for vulnerabilities
- Check database queries for SQL injection
- Validate API endpoints against OWASP
- Detect secrets in commits

## For those things, use VibeShield

VibeShield is a **satellite skill** that VibeDev invokes via handoff (`vibedev/references/handoffs.md`). The architecture is intentional: separation of concerns.

If you want VibeDev to detect security issues, you actually want VibeShield. The handoff between them is documented in `references/handoffs.md`.

## Why this is out of scope

Merging security auditing into VibeDev would:
- Bloat VibeDev's scope
- Couple two different concerns (governance + security)
- Make updates to one require releases of the other

The satellite pattern is the design. We are not changing it.
