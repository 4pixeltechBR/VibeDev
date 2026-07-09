# Changelog / Histórico de Mudanças

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),  
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [3.4.0] — 2026-07-11

### Added / Adicionado

- **VibeDev v1.5.0**: Layman total hardening + Modo País generic.
  - 🌍 **Modo País** generico — 12 países. Substitui `brasil.md`. See [release notes](./docs/releases/v3.4.0.md).
  - 👋 **Onboarding Tour** — 3 mensagens na primeira sessão pro leigo entender o framework.
  - 📊 **Painel Visual de Progresso** — `/vd-status` com barra visual ASCII pro leigo.
  - 🤝 **Decisão Confiante** — IA escolhe default pro leigo que não consegue avaliar opções técnicas.
  - 🛡️ **Validação Anti-"tá"** — `/vd-check` recusa aprovação vaga.
  - 🔄 **Recap Automático** — após 7+ dias fora, `/vd-status` expande em recap.
  - 💬 **Coleta Assistida de Features** — `/vd-plan` começa com 3 perguntas conversacionais.

### Changed / Modificado

- Modo País substituted Brasil file with generic 12-country system.
- SKILL.md indexed all references by category.

### Backward compatible / Sem quebra de compatibilidade

- BR funciona identicamente — só migrou de arquivo.
- Novas regras só ativam em modo leigo + sinais específicos.

[3.4.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.0

---

## [3.4.1] — 2026-07-11

### Changed / Modificado

- **`references/modo-pais.md` reorientado**: o framework é tecnologia cívica pra devs nativos de cada país construindo pro próprio país, com gateways/hosts/leis locais. Reclassificação de CN/RU/UA de "Experimental" pra **Estável (manutenção comunitária)**, com bloco "Lacunas conhecidas" em cada um. PRs de nativos especialmente bem-vindos.

### Backward compatible / Sem quebra de compatibilidade

- Mudança é de framing, não de comportamento técnico.

[3.4.1]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.1

---

## [3.4.2] — 2026-07-11

### Added / Adicionado

- **`CREDITS.md`** at repository root: formal, dated, traceable record of people, papers, projects, and communities that materially influenced VibeDev design. Includes section "How to add an entry" for community contributions. Refers cross-link from `CONTRIBUTING.md`.
- Hyperlinks to Sandeco Macedo's GitHub profile ([`@sandeco`](https://github.com/sandeco)) and the arXiv paper ([arXiv:2607.00038](https://arxiv.org/abs/2607.00038)) added across `docs/releases/v3.1.0.md`, `docs/releases/v3.2.0.md`, and `vibedev/CHANGELOG.md`.

[3.4.2]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.2

---

## [3.3.0] — 2026-07-10

### Added / Adicionado

- **VibeDev v1.4.0**: Closes the layman lifecycle — from vague idea to launched product.
  - ✨ **`/vd-spark`** — pre-Phase-1 discovery for laymen with vague ideas. See [release notes](./docs/releases/v3.3.0.md).
  - 🚀 **`/vd-launch`** — post-Phase-7 communication blocks (elevator pitch, tweet, LinkedIn, landing structure, beta email, private post-mortem) + 24h pre-launch checklist.
  - 🇧🇷 **Modo Brasil** — auto-activated when pt-BR / BR context detected. BRL, Pix-first, LGPD explicit, BR hosting options, pt-BR UI strings.

### Backward compatible / Sem quebra de compatibilidade

- `/vd-spark` purely additive — skips when intent is already clear.
- `/vd-launch` only triggers after Phase 7 → 8 gate (does nothing during construction).
- Modo Brasil opt-out by user request. International projects unaffected.
- All 9 originally identified layman gaps are now addressed across 3 minor releases.

[3.3.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.3.0

---

## [3.2.0] — 2026-07-09

### Added / Adicionado

- **VibeDev v1.3.0**: Quality-of-life additions for the long middle of projects.
  - ⏱️ **Estimativa de Tempo por Categoria** (`references/tempo-construcao.md`) — calibrates expectations before `/vd-start`. See [release notes](./docs/releases/v3.2.0.md).
  - 🛡️ **Anti-Feature-Creep** (`references/anti-creep.md`) — 3-layer scope containment during `/vd-build`. Ideas land in backlog, not in code.
  - 🎯 **Reconhecimento & Pausa** — embedded rituals for sober progress acknowledgment and pause prompts at phase gates. Anti-burnout, anti-guilt.

### Backward compatible / Sem quebra de compatibilidade

- All new behaviors are passive — they surface only when relevant. No command changed.
- Time estimates only appear in layman mode; technical mode users see no change in `/vd-start` flow.
- Recognition rituals are factual, one-line, opt-out by user request.

[3.2.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.2.0

---

## [3.1.0] — 2026-07-08

### Added / Adicionado

- **VibeDev v1.2.0**: Layman-friendly additions for non-technical users building their first AI-assisted project.
  - **Glossário Ativo** (`references/glossario-leigo.md`) — auto-translates jargon into plain language whenever `modo_usuario: leigo` is set. See [release notes](./docs/releases/v3.1.0.md).
  - **Guarda de Custo** — mandatory cost estimation (3 user tiers in BRL/month) before leaving Phase 3. Reference table in `references/estimativa-custos.md`.
  - **`/vd-kill`** — authorized project termination command. Archives state into `IDEA_LOG.md`, never deletes. The anti-guilt command.
  - **`IDEA_LOG-template.md`** — template for archive of completed/archived projects.

### Backward compatible / Sem quebra de compatibilidade

- Existing projects in technical mode are unaffected. Layman Mode additions only activate under `modo_usuario: leigo`.
- All new commands and fields are opt-in. No file deletion or migration required.

[3.1.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.1.0

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