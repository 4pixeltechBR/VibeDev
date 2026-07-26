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

## [1.5.1] — 2026-07-11

### Changed / Modificado

- **Reorientação do `references/modo-pais.md`** — o framework **não** é pra empresas estrangeiras "operando" em outros mercados. É tecnologia cívica pra **devs nativos** de cada país construindo ferramentas pro próprio país com infra local.
- CN, RU e UA reclassificados de "Experimental" pra **Estável (manutenção comunitária)**. Mantida seção "Lacunas conhecidas — contribuidor local bem-vindo" em cada um desses 3 países, listando pontos que só nativos podem endurecer.
- Disclaimer reescrito: agora deixa claro que é ponto de partida pra devs nativos, não guia pra estrangeiros operando em outros países.
- Matriz de decisão rápida ampliada: cada país agora tem sugestão específica em vez de casos hipotéticos.

### Backward compatible / Sem quebra de compatibilidade

- Mudança é de framing + reclassificação, não de comportamento técnico. `modo-pais.md` carrega igual.
- Projetos BR / US / EU não notam diferença.

[1.5.1]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.1

---

## [1.5.2] — 2026-07-11

### Added / Adicionado

- Hyperlinks to [Sandeco Macedo's GitHub profile](https://github.com/sandeco) and [arXiv:2607.00038](https://arxiv.org/abs/2607.00038) added throughout credit sections.

[1.5.2]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.4.2

---

## [1.6.0] — 2026-07-24

### Added / Adicionado

- **`/vd-help` command** (`vibedev/commands/vd-help.md`). Router que mapeia os 11 comandos do VibeDev + situações emocionais + fluxo principal resumido. Inspirado em `ask-matt` do mattpocock/skills (referência competitiva), adaptado pra leigo com mapa emocional e atalhos de emergência. User-invoked.
- **`.agents/adr/`** directory with 6 Architecture Decision Records:
  - `0001-skill-monolitica.md` — por que VibeDev é 1 SKILL.md, não N skills
  - `0002-modo-pais-generico.md` — por que 1 arquivo com 12 países, não 1 por país
  - `0003-modos-leigo-tecnico.md` — por que 2 modos (não 3+)
  - `0004-validacao-anti-ta.md` — por que /vd-check recusa "tá"
  - `0005-citacao-sandeco-formal.md` — por que CREDITS.md existe
  - `0006-model-invoked-anunciado.md` — por que anunciamos regras ao invés de silent
- **`.out-of-scope/`** directory with 7 documents explaining what VibeDev does NOT do and why:
  - `not-a-ci-cd-tool.md`, `not-for-monorepos.md`, `not-an-issue-tracker.md`, `not-a-low-code-builder.md`, `not-replacement-for-vibeshield.md`, `not-providing-stack-recommendations.md`, `not-pretending-to-be-vibeshield.md`

### Changed / Modificado

- `SKILL.md` agora tem seção **"Modelo de invocação"** com tabela user-invoked vs model-invoked-anunciado por comando. Decisão tomada em v3.4.0 ("Decisão Confiante") aplicada sistematicamente.
- `CREDITS.md` agora cita mattpocock/skills como **referência competitiva** (não paper acadêmico), com lista explícita do que foi adotado e do que foi rejeitado.

### Backward compatible / Sem quebra de compatibilidade

- `/vd-help` é puramente aditivo (novo comando, não substitui nenhum).
- ADRs e out-of-scope são apenas documentação. Não mudam comportamento.
- Mudança de invocação (silent → announced) é **transparente** pro leigo: ele recebe a mesma automação, só agora **vê** quando rola.

[1.6.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.5.0

---

## [1.7.0] — 2026-07-24

### Changed / Modificado

- `.changeset/` directory added at repo root with `config.json` (Changesets workflow), `README.md` (how-to), and `initial-setup.md` (first changeset for v3.6.0). Inspired by mattpocock/skills using same tool. **Workflow not yet automated** — humans add changesets during PRs; bot is future work.

[1.7.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.6.0

---

## [1.8.0] — 2026-07-25

### Added / Adicionado

- **`/vd-arch-review` command** (`vibedev/commands/vd-arch-review.md`). 3ª skill satélite da família VibeDev, user-invoked, para devs. 3 sub-comandos:
  - `/vd-arch-full` — macro + meso + micro (stack, arquitetura, saúde)
  - `/vd-arch-only` — só meso (arquitetura + componentes + fluxos)
  - `/vd-arch-health` — só micro (saúde + débito técnico)
- **3 reference prompts** (`vibedev/references/arch-review-{macro,meso,micro}.md`) com listas concretas de verificação por nível. Inspirado na escada de 5 níveis de Sandeco Macedo (arXiv:2607.00038) e em `improve-codebase-architecture` de mattpocock/skills.
- **2 templates** (`vibedev/assets/ARCH_MAP-template.md` + `vibedev/assets/arch-review-template.md`) para o documento mestre e os sub-relatórios.
- **2 ADRs** (`.agents/adr/0008-vd-arch-review-skill.md` + `.agents/adr/0009-arch-review-multi-nivel.md`).
- **`.out-of-scope/not-an-arch-audit-tool.md`** — explicita o que `/vd-arch-review` NÃO faz (não substitui architect, não refatora, não roda testes, etc).

### Changed / Modificado

- `SKILL.md` agora tem 12 comandos. Tabela de invocação atualizada.
- `commands/vd-help.md` menciona o 12º comando e indica que leigos podem ignorar.
- `CREDITS.md` agora cita `codebase-design` + `improve-codebase-architecture` de Matt como vocabulário referenciado.

### Backward compatible / Sem quebra de compatibilidade

- `/vd-arch-review` é puramente aditivo. Quem não chamar, não sente diferença.
- Não escreve em `PROJECT_STATE.md` (estado do projeto é governado por VibeDev). Escreve em `ARCH_MAP.md` próprio.
- **Não** ativa modo leigo. User-invoked, opt-in.
- G6 da VibeShield **não** dispara `/vd-arch-review` (decisão explícita: arquitetura é chamada humana, não auto).

[1.8.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.7.0

---

## [1.9.0] — 2026-07-26

### Added / Adicionado

> 🚀 **v3.8.0 — Orquestração em camadas.** 3 features coordenadas que fecham o ciclo de governança: rastreabilidade, pré-mortem e onboarding. Inspirado no Prompt Mestre WP de Dante Testa (jul/2026), mas open source e stack-agnostic.

- **Feature 1 — Assinatura de autoria opcional em arquivos gerados** (`vibedev/references/autoria-arquivos.md`).
  - Bloco `## Autoria` adicionado aos 3 templates de `PROJECT_STATE.md` (green, green-leigo, red).
  - Quando opt-in, cada arquivo gerado pelo agente recebe header de assinatura com nome, email, projeto, data, licença.
  - Formato do header adapta à linguagem: `//` (JS/TS/Go/PHP), `#` (Python/Ruby/Shell), `<!-- -->` (HTML/MD), `--` (SQL/Lua), `(* *)` (Pascal).
  - Formatos sem comentário (JSON, CSV, binário) recebem companion `.meta.json`.
  - `.env.example` NUNCA recebe header (risco de vazamento).
  - Trilha Vermelha tem flag adicional `[NOVO]` para arquivos novos vs `[EDITADO]` para modificados.
  - Compliance: LGPD Art. 37 (Brasil) + EU AI Act Art. 12 (Europa).
  - Comportamento default: zero header. Opt-in estrito.

- **Feature 2 — Advogado do Diabo como gate no `/vd-plan`** (`vibedev/commands/vd-devils-advocate.md`).
  - Novo sub-comando dispara automaticamente como gate no passo 4.5 do `/vd-plan` (após Red Team, antes de estimativa).
  - Gera 3 entregas obrigatórias: top 3 riscos (prob × impacto), 1 contradição interna, 1 ângulo cego.
  - Bloqueia progress até resposta do usuário nos 5 itens.
  - Opt-out via flag `--skip-devils-advocate`. Auto-skip em decisões triviais.
  - Inspirado em Gary Klein (HBR 2007) "Performing a Project Premortem" + Atul Gawande (2009) "The Checklist Manifesto".

- **Feature 3 — MANUAL.md automático no `/vd-close`** (`vibedev/commands/vd-manual.md`).
  - Novo sub-comando dispara automaticamente como gate no passo 4.5 do `/vd-close` (após compactação, antes da confirmação).
  - Gera/atualiza `MANUAL.md` com 5 seções: Visão geral, Como rodar, Como usar, Como estender, Troubleshooting.
  - Adapta ao `modo_usuario` (leigo: linguagem simples, sem paths absolutos; técnico: paths absolutos, classes, funções).
  - Preserva seções customizadas se `MANUAL.md` já existe.
  - Opt-out via flag `--no-manual`.

- **ADR 0010** (`.agents/adr/0010-v3-8-orquestracao-camadas.md`): documenta contexto, decisão, consequências e alternativas rejeitadas das 3 features.

### Changed / Modificado

- `SKILL.md` agora tem passo 4.5 no `/vd-plan` (gate do Advogado do Diabo) e passo 4.5 no `/vd-close` (gate do MANUAL).
- 3 templates de `PROJECT_STATE.md` ganharam bloco `## Autoria` (~10 linhas cada).
- Tabela de invocação implícita atualizada: `/vd-plan` agora tem 1 sub-comando, `/vd-close` agora tem 1 sub-comando.

### Backward compatible / Sem quebra de compatibilidade

- Bloco `## Autoria` em `PROJECT_STATE.md` é opt-in. Quem não preencher, comportamento é zero (zero header).
- `/vd-devils-advocate` tem opt-out via flag. Decisões triviais auto-skip.
- `/vd-manual` tem opt-out via `--no-manual`.
- Total de comandos formais: continua 12. Sub-comandos (4.5) são internos.

### Inspired by / Inspirado por

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026) — features 1 e 3.
- **Gary Klein — "Performing a Project Premortem"** (HBR, set/2007) — feature 2.
- **Atul Gawande — "The Checklist Manifesto"** (2009) — feature 2.
- **LGPD Art. 37** (Brasil, 2020) — feature 1.
- **EU AI Act Art. 12** (Europa, 2024) — feature 1.

[1.9.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.8.0

---

## [2.0.0] — 2026-07-26

### Added / Adicionado

> 🚀 **v3.9.0 — Motor de Fundação Técnica.** Novo sub-comando `/vd-scaffold` que roda DENTRO de `/vd-build` quando a Trilha é Verde, a fase é **Arquitetura**, e a sub-tarefa envolve fundação (DB, auth, pagamento, multi-tenant, white-label). **Stack é decisão Tipo 1** — nunca fixa. Inspirado no Prompt Mestre WP da Dante Testa, mas open source, stack-agnostic, e integrado com a v3.8 (Advogado do Diabo dispara quando triagem discorda do `PROJECT_STATE`).

- **`/vd-scaffold` sub-comando** (`vibedev/commands/vd-scaffold.md`). Ativado automaticamente como gate no `/vd-build` (passo pós-Aprovação, pré-codificação). Triagem Cynefin-lite (3 perguntas máximo) + Red Team + 4 componentes reais gerados.

- **6 references do scaffold:**
  - `vibedev/references/scaffold-stack-validada.md` — 3 stacks por perfil Cynefin (Complicado/Simples/Caótico) + detalhamento FastAPI+React+TS+Postgres, Next.js+SQLite, e caso de borda (legado).
  - `vibedev/references/scaffold-schema-migrations.md` — schema agnóstico com 6 regras obrigatórias (id, created/updated, soft delete, índices, FK com ondelete, multi-tenancy) + exemplos em Alembic e Drizzle.
  - `vibedev/references/scaffold-rbac.md` — matriz RBAC 4-dimensional (Role × Recurso × Ação × Escopo), template + 3 opções de implementação (coluna simples, tabela de permissões, Casbin/Oso).
  - `vibedev/references/scaffold-pagamento-br.md` — pagamento **BR-first** (PIX + Mercado Pago + Asaas como primários, Stripe como fallback). Webhook idempotente obrigatório (UNIQUE em `(provider, event_id)`). Reconciliação de PIX pendente >1h. Compliance LGPD Art. 46 + PCI-DSS 4.0.
  - `vibedev/references/scaffold-white-label.md` — tokens parametrizados por tenant (cores, fontes, logo, favicon, domínio, email de suporte). 3 opções (CSS vars, Tailwind, Emotion). Proibido hardcode (gate H2 sugerido).
  - `vibedev/references/scaffold-legado-coexistencia.md` — caso de borda Cynefin (WordPress, ERP, planilha). Sempre tratado como Tipo 1 de alta incerteza. 3 opções: manter legado, migrar tudo, sincronização bidirecional (Strangler Fig + CDC).

- **Gate do Advogado do Diabo (v3.8) estendido para scaffold:** se `Stack decidida` em `PROJECT_STATE.md` está preenchida MAS a triagem sugere outra, dispara `/vd-devils-advocate` com a tensão como contradição interna. Tensão stack registrada → decisão consciente, não automática.

- **Métrica `stack_trocada`** no `PROJECT_STATE.md`: incrementa se a stack escolhida na Triagem for revertida depois. Sinal de que a Triagem está errando — se subir, revisar as 3 perguntas antes de revisar o código.

### Changed / Modificado

- `SKILL.md` agora tem 13 comandos (12 + `/vd-scaffold` como sub-comando). Tabela de invocação atualizada. Gate explícito no `/vd-build`.
- 3 templates de `PROJECT_STATE.md` ganham campo `stack_trocada` em `## Métricas do framework`.
- Lista de comandos (linha 11) inclui `/vd-scaffold`.

### Backward compatible / Sem quebra de compatibilidade

- `/vd-scaffold` só roda se Trilha = Verde + Fase = 3 + sub-tarefa envolve fundação. Em qualquer outro contexto, é no-op.
- Opt-out via flag `--skip-scaffold` no `/vd-build`.
- 3 famílias de skills (VibeDev, VibeShield, vd-arch-review) inalteradas. `/vd-scaffold` é **sub-comando de VibeDev**, não 4ª skill.
- Compatível com v3.8 (Advogado do Diabo) — integra, não substitui.

### Inspired by / Inspirado por

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026) — gerador de fundação com pagamento próprio e webhook idempotente. Esta release é a versão open source e stack-agnostic.
- **Cynefin Framework** (Dave Snowden, 1999) — base da Triagem Complicado/Simples/Caótico.
- **Strangler Fig Pattern** (Martin Fowler, 2004) — base da coexistência com legado.
- **Change Data Capture** (Debezium, 2015+) — base da sincronização bidirecional.
- **PCI-DSS 4.0** (2024) — base de segurança de pagamento.
- **LGPD Art. 46** (Brasil, 2020) — base de privacidade em pagamento.
- **CSS Custom Properties** — base de white-label agnóstico.

[2.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.9.0

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
- **Anti-feature-creep guard** (`references/anti-creep.md`). 3-layer containment for scope creep: pre-build scope check vs anti-scope + done criterion; real-time backlog registration during `/vd-build`; mandatory pause for high-impact additions. Inspired by Macedo's "specification gaming" anti-pattern ([arXiv:2607.00038](https://arxiv.org/abs/2607.00038)).
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