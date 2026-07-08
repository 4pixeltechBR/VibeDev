# Changelog — VibeDev

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.2.0] — 2026-07-08

### Added / Adicionado

- **Active Layman Glossary** (`references/glossario-leigo.md`). Auto-loaded whenever `modo_usuario: leigo` is detected in `PROJECT_STATE.md`. Replaces technical jargon with plain-language translations in ALL outputs (status, plan, gate, check, conversation). New terms encountered are translated inline and appended to the glossary silently.
- **Cost Guard** (regra universal na `SKILL.md`). Mandatory cost estimation table (3 user tiers: 1-10, 10-100, 100-1000) in BRL/month before promoting from Phase 3 to Phase 4. Backed by `references/estimativa-custos.md`, a reference table by category (landing, site, internal app, public SaaS, bot, mobile) and user tier.
- **`/vd-kill` command** (autorizado encerramento). Graceful project termination with dignity. Archives the state into `IDEA_LOG.md` with reflective questions (`what problem I really thought it solved / what I learned / smaller path next`), renames the original state file, never deletes. The anti-guilt of VibeDev.
- **`IDEA_LOG-template.md`** (`assets/IDEA_LOG-template.md`). Template for the archive of archived projects.

### Changed / Modificado

- `SKILL.md` boot protocol: now reads `modo_usuario` field and switches rendering mode automatically (leigo vs tecnico). Layman Mode renders 3 simple lines on `/vd-status` instead of technical 4-line format.
- `assets/PROJECT_STATE-green-leigo.md`: new mandatory section `Custo mensal estimado` with 3 user tiers. Gate Phase 3 → 4 is BLOCKED until populated (or user explicitly skips with log entry).

### Backward compatible / Sem quebra de compatibilidade

- Projects using technical mode (`modo_usuario: tecnico` or absent) are unaffected. New fields are opt-in.
- The `custo_mensal_estimado` field is mandatory only for new Layman Mode projects. Existing ones can adopt it at next phase transition.
- `/vd-kill` is a new command — doesn't modify existing commands. Safe to ignore if never used.
- Glossary only triggers under `modo_usuario: leigo`. Technical mode outputs unchanged.

[1.2.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.1.0

---

## [1.1.0] — 2026-06-30

### Added / Adicionado

- `references/handoffs.md` - dispatch bridge to satellite skills. Defines mode detection protocol (`modo_usuario`), list of satellite skills with compact format (who/when/what/how-to-integrate), and cross-references to VibeShield's `references/handoff-vibedev.md` (canonical).
- `assets/PROJECT_STATE-green-leigo.md` - Layman Mode state template. 5 phases instead of 8, each phase described in natural language, no engineering jargon, focused on `modo_usuario: leigo` from project birth.
- Documentation updated in `SKILL.md` (`/vd-start` command):
  - Template selection table by user profile.
  - How to ask in natural language if the user has engineering background.
  - Adaptation of `trilha-verde.md` content to layman language when layman template is chosen.

### Changed / Modificado

- `SKILL.md` (`/vd-start`): now detects user profile and selects appropriate template between `PROJECT_STATE-green.md` (technical) or `PROJECT_STATE-green-leigo.md` (layman). Previously, only the technical template existed.

### Backward compatible / Sem quebra de compatibilidade

- Previous versions of projects with `PROJECT_STATE.md` in technical template keep working.
- The `modo_usuario` field is opt-in. If absent, VibeShield assumes Technical Mode (safe fallback).
- VibeShield as satellite remains optional - projects that don't use VibeShield are unaffected.

---

## [1.0.0] - 2025-XX-XX

### Added / Adicionado (initial release)

- Initial release of VibeDev as governance framework for AI-assisted development.
- Commands: `/vd-start`, `/vd-status`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-close`, `/vd-compact`.
- State templates: `PROJECT_STATE-green.md`, `PROJECT_STATE-red.md`, `PROJECT_STATE_ARCHIVE-template.md`.
- References: `gates.md`, `stack-guide.md`, `trilha-verde.md`, `trilha-vermelha.md`.
- Persona: Architect-Guardian.
- Core principle: state lives in file, never in conversation memory.

[1.1.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.1.0
[1.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.0.0