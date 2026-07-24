# VibeShield does not cover runtime race conditions

VibeShield performs **static analysis** of the code visible in the project state. It does not run the code, simulate concurrent access, or detect race conditions.

## What VibeShield does
- Reads the source code in the project
- Identifies patterns matching known anti-patterns per category
- Returns verdicts based on what is **readable** in the code

## What VibeShield does NOT do
- Detect time-of-check vs time-of-use (TOCTOU) race conditions
- Simulate concurrent user access
- Identify deadlocks in async code
- Find memory leaks or buffer overflows
- Run the code in a sandbox
- Profile runtime performance

## For runtime safety, use
- **Race condition detectors** (Go race detector, ThreadSanitizer for C/C++)
- **Property-based testing** (QuickCheck, fast-check)
- **Load testing** (k6, Locust, Gatling)
- **Code review with concurrent user stories** in mind

## Why this is out of scope

VibeShield's value is **catching what the human can see in the code at review time**. Runtime race conditions require **execution context** that's not available in static analysis.

If a category touches concurrency (C4 Validation, C2 Authorization), VibeShield flags the *pattern* but does not guarantee runtime safety.
