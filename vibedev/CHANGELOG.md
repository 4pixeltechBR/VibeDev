# Changelog — VibeDev

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),  
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.1.0] — 2026-06-30

### Added / Adicionado

- `references/handoffs.md` — dispatch bridge to satellite skills. Defines mode detection protocol (`modo_usuario`), list of satellite skills with compact format (who/when/what/how-to-integrate), and cross-references to VibeShield's `references/handoff-vibedev.md` (canonical).
- `assets/PROJECT_STATE-green-leigo.md` — Layman Mode state template. 5 phases instead of 8, each phase described in natural language, no engineering jargon, focused on `modo_usuario: leigo` from project birth.
- Documentation updated in `SKILL.md` (`/vd-start` command):
  - Template selection table by user profile.
  - How to ask in natural language if the user has engineering background.
  - Adaptation of `trilha-verde.md` content to layman language when layman template is chosen.

### Changed / Modificado

- `SKILL.md` (`/vd-start`): now detects user profile and selects appropriate template between `PROJECT_STATE-green.md` (technical) or `PROJECT_STATE-green-leigo.md` (layman). Previously, only the technical template existed.

### Backward compatible / Sem quebra de compatibilidade

- Previous versions of projects with `PROJECT_STATE.md` in technical template keep working.
- The `modo_usuario` field is opt-in. If absent, VibeShield assumes Technical Mode (safe fallback).
- VibeShield as satellite remains optional — projects that don't use VibeShield are unaffected.

---

## [1.0.0] — 2025-XX-XX

### Added / Adicionado (initial release)

- Initial release of VibeDev as governance framework for AI-assisted development.
- Commands: `/vd-start`, `/vd-status`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-close`, `/vd-compact`.
- State templates: `PROJECT_STATE-green.md`, `PROJECT_STATE-red.md`, `PROJECT_STATE_ARCHIVE-template.md`.
- References: `gates.md`, `stack-guide.md`, `trilha-verde.md`, `trilha-vermelha.md`.
- Persona: Architect-Guardian.
- Core principle: state lives in file, never in conversation memory.

[1.1.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.1.0
[1.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.0.0