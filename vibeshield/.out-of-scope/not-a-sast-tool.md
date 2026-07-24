# VibeShield is not a SAST tool

VibeShield performs **opinionated, framework-specific code review** based on VibeDev's 8 categories. It is not a general-purpose static analysis security testing (SAST) tool.

## What VibeShield does
- Reviews the active sub-task's code for VibeDev-specific concerns (C1-C8)
- Returns verdicts tailored to the project's stack and phase
- Integrates with VibeDev's state file (`PROJECT_STATE.md`)
- Speaks in the language the user understands (technical OR leigo mode)

## What VibeShield does NOT do
- Replace dedicated SAST tools (Semgrep, Snyk, CodeQL, Bearer, SonarQube)
- Provide a comprehensive vulnerability database
- Detect every known CVE
- Generate compliance reports (SOC2, ISO 27001, PCI-DSS)
- Triage a codebase with no project state

## For comprehensive SAST, use
- **Semgrep** — open-source, customizable rules
- **Snyk Code** — commercial, broad coverage
- **GitHub CodeQL** — deep analysis for GitHub-hosted projects
- **SonarQube** — multi-language enterprise SAST
- **Bearer** — privacy-focused SAST

## Why this is out of scope

SAST tools and VibeShield serve **different layers**:

| Layer | Tool |
|---|---|
| Project governance (what to build, when) | VibeDev |
| Per-sub-task security review | **VibeShield** |
| Whole-codebase SAST scan | Semgrep, Snyk, etc. |
| Production monitoring | Sentry, Datadog |
| Penetration testing | OWASP ZAP, Burp Suite |

A mature project typically uses **all of these**, not one. VibeShield's place is between VibeDev's per-task cycle and the broader security tooling ecosystem.
