# 0008. `/vd-arch-review` como 3ª skill satélite (não na VibeDev core, não na VibeShield)

- **Status:** Accepted
- **Date:** 2026-07-25
- **Deciders:** Mavis + 4pixeltechBR

## Context

VibeDev cobre governança de projeto (8 fases, 11 comandos, estado em arquivo). VibeShield cobre auditoria de segurança (gatilhos G1-G7, categorias C1-C8, code-review). Faltava: **auditoria arquitetural de código** — separação de camadas, anti-patterns, dep vs custom.

Três caminhos possíveis:
- (A) Incorporar como 3ª skill satélite, user-invoked
- (B) Estender `vibeshield-code-review` (eixo Standards) com checklist arquitetural
- (C) Skill standalone `software-mapper` em `tools/`

## Decision

Opção A: criar `/vd-arch-review` como 3ª skill da família, user-invoked, sob demanda. Skill dedicada em `vibedev/commands/vd-arch-review.md` (mesmo padrão do `/vd-help`).

**Por que não VibeShield:** VibeShield é segurança (C1-C8). Arquitetura é ortogonal — separação de camadas não é auth, acoplamento não é XSS. Misturar os dois viola o princípio de "uma skill, uma responsabilidade" e dilui foco.

**Por que não VibeDev core:** SKILL.md principal é governança de projeto, não auditoria de código. Adicionar 12º comando polui sem agregar pra leigo (que é público-alvo principal).

**Por que não standalone:** Reutiliza CREDITS, out-of-scope, ADRs, handoffs. Tem audiência imediata (10 releases, GitHub ativo). Tem consistência com VibeShield (mesmo padrão).

## Consequences

- **Facilita:** fechamento dos 3 buracos (camadas, anti-patterns, dep vs custom), coesão com VibeShield, posicionamento claro no ecossistema
- **Dificulta:** adicionar 1 front de manutenção, fragmentar "lembrar qual skill chama"
- **Perdemos:** simplicidade de "1 skill que faz tudo", leigo não tem mais 1 framework

## Alternatives considered

- **Estender `vibeshield-code-review`** (eixo Standards ganha 3 categorias) — rejeitado: VibeShield vira "tudo de qualidade"
- **Skill standalone `software-mapper`** (subpasta `tools/`) — rejeitado: duplica Matt Pocock, abandona a família
- **Incorporar como 12º comando na VibeDev** — rejeitado: polui SKILL.md principal, quebra foco

## Invariantes

- User-invoked (humano chama). Não dispara automaticamente.
- Não escreve em `PROJECT_STATE.md` (estado do projeto é governado por VibeDev)
- Escreve em `ARCH_MAP.md` (raiz do projeto) + `.arch-review/` (subdir)
- Vocabulário de módulos/seams/adapters referenciado de Matt Pocock's `codebase-design`
- Documentado em CREDITS, .out-of-scope, ADRs (clareza de escopo)
