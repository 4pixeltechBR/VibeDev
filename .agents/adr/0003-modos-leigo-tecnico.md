# 0003. Dois modos (leigo + técnico), não três ou cinco

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

VibeDev detecta perfil do usuário via campo `modo_usuario` no `PROJECT_STATE.md`. Opções:
- **2 modos** (leigo + técnico) — atual
- **3+ modos** (leigo total, leigo com noção técnica, júnior, pleno, sênior) — granularidade extra
- **0 modos** (IA sempre adapta sem perguntar) — implícito, mas perde auditabilidade

## Decision

2 modos: `leigo` e `tecnico`. Default = `tecnico` (safe fallback). Leigo ativa explicitamente no `/vd-start`.

## Consequences

- **Facilita:** clareza de comportamento, fácil decidir, fácil documentar
- **Dificulta:** usuário intermediário (sabe programar pouco, entende termos básicos) não tem modo dedicado — fica no `tecnico` e se vira
- **Perdemos:** personalização fina pra cada perfil

## Alternatives considered

- **Granularidade por senioridade** — 5 modos seria overkill, explode templates
- **0 modos** — IA decide sozinha. Problema: leigo não sabe o que tá acontecendo
- **Auto-detecção por comportamento** — experimentamos mentalmente, mas adiciona complexidade e a primeira interação fica imprevisível
