# VibeShield does not audit transitive dependencies

VibeShield reviews **code written in the project**. It does not audit the entire dependency tree.

## What VibeShield does
- Reads the project's own source files
- Flags anti-patterns in the code the team wrote
- Returns verdicts on direct code changes

## What VibeShield does NOT do
- Audit every dependency in `package.json` / `requirements.txt` / `Cargo.toml`
- Detect known CVEs in third-party libraries
- Check license compliance (MIT, GPL, etc.)
- Analyze supply chain risk
- Generate Software Bill of Materials (SBOM)

## For dependency audit, use
- **`npm audit` / `yarn audit`** for JavaScript/Node
- **`pip-audit` / `safety`** for Python
- **`cargo audit`** for Rust
- **`govulncheck`** for Go
- **Snyk Open Source** for multi-language
- **Dependabot / Renovate** for automated PRs

## Why this is out of scope

VibeShield triggers on **G5 — new dependency enters the project** (see SKILL.md). At that point, it can flag the dependency for review, but the **comprehensive audit** of the full tree is a different problem with different tools.

The pattern: VibeShield says "G5 fired, new dep, recommend running `npm audit` before continuing". The actual `npm audit` is the project's responsibility, not VibeShield's.
