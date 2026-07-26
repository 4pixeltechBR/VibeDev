# 0010. v3.8 — Orquestração em camadas (autoria + advogado + manual)

- **Status:** Accepted
- **Date:** 2026-07-26
- **Deciders:** Mavis + 4pixeltechBR

## Context

A v3.7.0 introduziu o `/vd-arch-review` como satélite. Identificamos que
a experiência do usuário tem 3 gaps recorrentes ao longo do ciclo de vida
de um projeto:

1. **Proveniência perdida:** arquivos gerados pela IA não têm rastreabilidade
   sobre quem/quando/por quê foram criados. LGPD Art. 37 e EU AI Act Art. 12
   pedem rastreabilidade. Soluções pagas (Dante Testa) já implementam
   assinatura, mas a VibeDev não.
2. **Decisões sem pré-mortem:** o `/vd-plan` quebra uma intenção em sub-tarefas,
   mas o Red Team é aplicado apenas na opção recomendada. Riscos não-menção-
   dos pelo usuário passam batido. Gary Klein (HBR 2007) e Atul Gawande (2009)
   mostram que checks simples antes de ação reduzem falhas dramaticamente.
3. **Onboarding custoso:** ao encerrar sessão (`/vd-close`), o estado é
   persistido, mas o usuário (especialmente leigo) não recebe um manual
   prático. Na próxima sessão, ou ele lê o `PROJECT_STATE.md` inteiro, ou
   não sabe como rodar/usar o que foi feito.

## Decision

Adicionar 3 features coordenadas na v3.8.0:

### 1. Autoria opcional (Feature 3 do plano de v3.8)
- Bloco `## Autoria` em todos os 3 templates de `PROJECT_STATE.md` (green,
  green-leigo, red).
- Reference `autoria-arquivos.md` define comportamento por linguagem.
- Header automático em arquivos novos quando opt-in.
- Trilha Vermelha tem flag adicional `[NOVO]` vs `[EDITADO]`.
- `.env.example` NUNCA recebe header (risco de vazamento).

### 2. Advogado do Diabo no `/vd-plan` (Feature 2)
- Novo sub-comando `vd-devils-advocate.md` (gate no passo 4.5 do `/vd-plan`).
- Roda após Red Team, antes de estimativa.
- Gera: top 3 riscos (prob × impacto) + 1 contradição interna + 1 ângulo cego.
- Bloqueia progress até resposta do usuário nos 5 itens.
- Opt-out via `--skip-devils-advocate`. Auto-skip em decisões triviais.

### 3. MANUAL.md automático no `/vd-close` (Feature 4)
- Novo sub-comando `vd-manual.md` (gate no passo 4.5 do `/vd-close`).
- Gera/atualiza `MANUAL.md` com 5 seções: Visão geral, Como rodar, Como usar,
  Como estender, Troubleshooting.
- Adapta ao `modo_usuario` (leigo vs técnico).
- Preserva seções customizadas se `MANUAL.md` já existe.
- Opt-out via `--no-manual`.

## Consequences

### Easier
- Rastreabilidade nativa de arquivos gerados por IA (compliance-friendly).
- Decisões Tipo 1 têm pré-mortem obrigatório (reduz falhas caras).
- Onboarding de projetos mantidos pela VibeDev é trivial (MANUAL.md).
- VibeDev vira alternativa open source a produtos pagos como Prompt Mestre WP.

### Harder
- `/vd-plan` fica mais lento (gate adiciona ~1-2 min em decisões Tipo 1).
- `/vd-close` gera mais um arquivo (MANUAL.md).
- Templates de PROJECT_STATE.md crescem ~10 linhas (autoria).
- Usuários apressados vão usar `--skip-devils-advocate` em tudo (anti-padrão).

### Given up
- Sem gate do Advogado do Diabo em decisões Tipo 2 (reversíveis). Tradeoff
  explícito: velocidade > rigor em decisões baratas.
- Sem geração de diagramas no MANUAL.md em modo leigo (complexidade técnica).
- Sem assinatura criptográfica (PGP, sigstore) — só header textual. Tradeoff
  explícito: simplicidade > criptografia forense.

## Alternatives considered

### A) Tudo em uma única feature "orquestração"
Rejeitado. Cada feature tem objetivo distinto (rastreabilidade vs pré-mortem
vs onboarding). Acoplar dificulta opt-out e esconde intenção.

### B) Advogado do Diabo como comando independente (não gate)
Rejeitado. Se for comando separado, usuário vai pular em 80% dos casos
(lei do menor esforço). Como gate, é opt-out explícito, mais difícil de
ignorar por acidente.

### C) MANUAL.md gerado manualmente pelo agente (sem sub-comando)
Rejeitado. Inconsistência de formato entre projetos. Sem versionamento
automático. Sem adaptação a modo_usuario.

### D) Implementar as 3 features em releases separadas (v3.8.0, v3.8.1, v3.9.0)
Rejeitado. As 3 formam um conjunto coeso de "orquestração em camadas" —
faz sentido semanticamente em uma única release minor (v3.8.0).

## References

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026) — features 1 e 3
- **Gary Klein — "Performing a Project Premortem"** (HBR, set/2007) — feature 2
- **Atul Gawande — "The Checklist Manifesto"** (2009) — feature 2
- **LGPD Art. 37** (Brasil, 2020) — feature 1
- **EU AI Act Art. 12** (Europa, 2024) — feature 1
- **CREDITS.md** — Sandeco Macedo (arXiv:2607.00038), Matt Pocock/skills
- **Sandeco Macedo — 5-level verification ladder** (jul/2026) — base conceitual
  do pré-mortem em camadas

## Implementation commits

- `7990c81` — feat(v3.8): assinatura de autoria opcional em arquivos gerados
- `dd7db35` — feat(v3.8): advogado do diabo como gate no /vd-plan
- `3f648f4` — feat(v3.8): MANUAL.md automatico no /vd-close
