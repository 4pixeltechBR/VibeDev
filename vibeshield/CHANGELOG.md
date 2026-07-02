# Changelog — VibeShield

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),  
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.0.0] — 2026-06-30

### Added / Adicionado

- `SKILL.md` — entry point with non-negotiable principles, G1-G7 triggers, 3 canonical verdicts, envelope format.
- `references/handoff-vibedev.md` — canonical contract with VibeDev. Includes:
  - Closed vocabulary (active sub-task, Type 1/Type 2 decision, G# trigger, etc.).
  - VibeDev → satellite trigger protocol.
  - Satellite skill execution protocol.
  - Output envelope format.
  - 8 canonical categories (C1-C8) with OWASP Top 10 mapping.
  - 3 canonical verdicts (OK / REVIEW / BLOCK).
  - State access rules (append-only, ISO timestamp).
  - 5 error situations (E1-E5).
  - **Layman Mode** (section 0): automatic detection via `modo_usuario` in state, safe fallback.
- `references/glossario-leigo.md` — C1-C8 translation dictionary → natural language question, with 3 recommended options per category and anchor phrases.
- `references/formato-conversa-leigo.md` — 3 conversational output templates (Decision Needed, Quick Notice, Help Conversation) + opening, closing, fallback rules, 3-minute rule.
- `references/estado-visual.md` — visual layer that renders `PROJECT_STATE.md` in summarized panel for layman.
- `examples/auth-google.md` — end-to-end walkthrough of Login with Google OAuth showing both modes (Technical + Layman) in parallel. Example stack: Node.js / Express.

### Established principles / Princípios estabelecidos

1. Satellite skill has no hands (analysis only).
2. Satellite skill does not decide for the user.
3. C1-C8 categories are closed vocabulary.
4. BLOCK verdict is real blocking, not advice.
5. Shared state via `PROJECT_STATE.md`, append-only mode.

[1.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.0.0