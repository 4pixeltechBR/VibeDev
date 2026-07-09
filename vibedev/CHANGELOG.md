# Changelog — VibeDev

All notable changes to this project are documented here.  
Todas as mudanças notáveis neste projeto são documentadas aqui.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.5.0] — 2026-07-11

### Added / Adicionado

- **Modo País genérico** (`references/modo-pais.md`). Substitui `brasil.md` e cobre 12 países com detecção por sinais combinados (TLD + moeda + idioma + gateway + fuso): 🇧🇷 BR, 🇵🇹 PT, 🇺🇸 US, 🇬🇧 GB, 🇲🇽 MX, 🇦🇷 AR, 🇨🇱 CL, 🇨🇳 CN, 🇷🇺 RU, 🇺🇦 UA, 🇪🇺 EU, 🇯🇵 JP. CN/RU/UA marcados como **experimental** com disclaimer jurídico reforçado (PIPL + 152-ФЗ + conflito armado).
- **Onboarding Tour** (`references/onboarding-leigo.md`). 3 mensagens curtas mostrado **só na primeira sessão** de projeto novo em modo leigo. Reconhece a chegada, mapeia 5 fases visualmente, oferece escolha binária (zero vs bagunça).
- **Painel Visual de Progresso** (`references/painel-progresso.md`). Render alternativo do `/vd-status` com barra de progresso, trilha mapeada, conquistas e custo. Ativado em modo leigo, em ASCII puro (sem cor).
- **Decisão Confiante por Padrão** (em `SKILL.md`). Quando leigo não consegue avaliar opções técnicas, IA escolhe a recomendada com justificativa de 1 linha + registra delegação. Evita "escolhe você" virar padrão silencioso.
- **Validação Anti-"tá"** (no `/vd-check`). Respostas vagas ("tá", "deve tá", "acho que sim") **não** contam como aprovação. Reformula com evento observável específico.
- **Recap Automático** (`references/recap-automatico.md`). Quando leigo volta 7+ dias depois, `/vd-status` expande em recap + 3 opções (continuar / revisar / replanejar). 90+ dias → sugere `/vd-kill` ou redescobrir.
- **Coleta Assistida de Features** (`references/coleta-features-assistida.md`). Substitui pedido aberto "liste 10 features testáveis" por 3 perguntas conversacionais (fluxo principal / o que não quero / anti-escopo). Alimenta `/vd-plan` formal automaticamente.

### Removed / Removido

- `references/brasil.md` — substituído por `modo-pais.md` (que contém BR como seção).

### Changed / Modificado

- `SKILL.md` agora referencia o conjunto completo de referências em seção própria, organizada por categoria (Trilhas / Layman Mode / Modo País / Launch / Integrações).
- `/vd-status`, `/vd-plan`, `/vd-check` documentados com modo leigo e regras de auto-ativação.

### Backward compatible / Sem quebra de compatibilidade

- BR continua funcionando idêntico — só migrou de arquivo.
- Onboarding só roda primeira sessão; projetos existentes não veem tour.
- Validação reforçada afeta **só** o que entrar no `/vd-check`. Builds não mudam.
- Recap depende do campo `ultima_sessao_em` que é opt-in (adicionado pelo `/vd-close`).
- Coleta assistida só ativa se `modo_usuario: leigo`. Devs mantém fluxo padrão.

[1.5.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.0

---

## [1.4.0] — 2026-07-10

### Added / Adicionado

- **`/vd-spark` command** + `references/discovery-leigo.md`. Four-round conversation that extracts (idea / persona / transformation / success signal) from a layman who arrives with a vague Instagram-reel idea. Auto-activates when project is new and intent is ambiguous. Output: `discovery_brief` block that feeds the formal `/vd-start`. Doesn't replace `/vd-start` — precedes it.
- **`/vd-launch` command** + `assets/launch/launch-brief-template.md` + `assets/launch/checklist-pre-launch.md`. Generates the communication blocks after gate Phase 7 → 8: elevator pitch, 3 tweet variants, LinkedIn post, landing page structure, personal email to first 10 beta users, private post-mortem. IA does NOT build the landing page itself — delivers blocks the human edits.
- **Modo Brasil** (auto-activation by context) + `references/brasil.md`. Loaded when session is pt-BR or project has BR context. Activates: BRL as primary currency, Pix-first payment suggestion, explicit LGPD in security checks, BR hosting option prompt, pt-BR UI strings, DD/MM/AAAA dates, GMT-3 timezone.

### Changed / Modificado

- `SKILL.md` boot protocol: now recognizes pt-BR session / BR context signals and auto-loads `references/brasil.md` next to the Layman Glossary.
- `SKILL.md` glossary section now includes **Modo Brasil** as a passive auto-activation rule.

### Backward compatible / Sem quebra de compatibilidade

- `/vd-spark` is purely additive — skip if user arrives with clear scope.
- `/vd-launch` only triggers after Phase 7→8 gate (does nothing during construction).
- Modo Brasil auto-activates but can be opted out. Projects in English / for international market are unaffected.
- New templates live under `assets/launch/` and `assets/` — don't conflict with existing state templates.

[1.4.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.3.0

---

## [1.3.0] — 2026-07-09

### Added / Adicionado

- **Calibrated construction-time estimates** (`references/tempo-construcao.md`). Realistic time ranges by project category (landing, site, internal app, public SaaS, bot, mobile app) and user profile (dev vs leigo, dedicated vs spare-time). Loaded automatically during `/vd-start` in layman mode **before** formal diagnosis. Goal: align expectations with reality, not promise Instagram-reel timelines.
- **Anti-feature-creep guard** (`references/anti-creep.md`). 3-layer containment for scope creep: pre-build scope check vs anti-scope + done criterion; real-time backlog registration during `/vd-build`; mandatory pause for high-impact additions. Inspired by Macedo's "specification gaming" anti-pattern (arXiv:2607.00038).
- **Recognition & Pause** rule (in `SKILL.md`). Three small embedded rituals: factual micro-progress every 3 sub-tasks, sober celebration + mandatory pause prompt at phase gates, anti-guilt language for "how much longer?" questions. Acknowledges that building alone is exhausting work; the framework should not pretend otherwise.

### Backward compatible / Sem quebra de compatibilidade

- Construction-time estimates only surface in layman mode and only at `/vd-start`. Doesn't slow down technical-mode flow.
- Anti-creep guard is automatic — doesn't change any existing command, only adds new behavior inside them.
- Recognition ritual is non-intrusive: factual, one-line, no emoji. Skip in technical mode if user prefers pure signal.

[1.3.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.2.0

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