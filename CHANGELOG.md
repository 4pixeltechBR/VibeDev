# Changelog / Histórico de Mudanças

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),  
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [3.0.0] — 2026-07-02

### Added / Adicionado

- **Bilingual root README** (EN + PT-BR in a single file). / **README bilíngue** (EN + PT-BR em arquivo único).
- Full ecosystem repository structure with `vibedev/` and `vibeshield/` folders. / Estrutura completa de repositório de ecossistema com pastas `vibedev/` e `vibeshield/`.
- `INSTALL.md` with multi-environment installation instructions (Claude Code, Cursor, Antigravity, OpenCode, Codex). / `INSTALL.md` com instruções de instalação multi-ambiente.
- `MANUAL.md` with complete usage guide (cycle, triggers, modes, troubleshooting). / `MANUAL.md` com guia completo de uso.
- `CONTRIBUTING.md`, `.gitignore`, `.gitattributes`, `.github/` templates for issues and PRs. / Templates de issues e PRs.
- `LICENSE` (MIT).

### Restored / Restaurado

After a repository reset that kept only the root README, this release restores the complete file structure with all skill content.

Após um reset do repositório que manteve apenas o README raiz, esta release restaura a estrutura completa de arquivos com todo o conteúdo das skills.

---

## [1.0.0] — 2026-06-30

### Added / Adicionado (initial release)

- **VibeDev v1.1.0**: Framework de governança com 8 fases (Verde) / 5 fases (Vermelha), 7 comandos (`/vd-start`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-close`, `/vd-status`, `/vd-compact`), suporte a `modo_usuario: leigo` com template dedicado.
- **VibeShield v1.0.0**: Skill-satélite de auditoria de segurança com 8 categorias (C1-C8), 7 gatilhos (G1-G7), 3 verdicts (OK / REVISAR / BLOQUEAR), 3 templates de saída para Modo Leigo, painel visual, glossário embutido.
- **Handoff canônico VibeDev ↔ VibeShield** em `vibeshield/references/handoff-vibedev.md`.
- **Ponte de despacho** em `vibedev/references/handoffs.md`.
- **Exemplo end-to-end** de login com Google OAuth em `vibeshield/examples/auth-google.md`.

[3.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.0.0
[1.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.0.0