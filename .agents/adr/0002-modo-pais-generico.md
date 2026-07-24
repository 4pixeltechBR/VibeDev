# 0002. Modo País genérico em vez de um arquivo por país

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

VibeDev tem `references/modo-pais.md` cobrindo 12 países. Alternativa: 1 arquivo por país (`pais-BR.md`, `pais-RU.md` etc).

## Decision

1 arquivo único com seções por país, auto-detecção por sinais combinados (TLD + moeda + idioma + gateway + fuso), opt-out explícito.

## Consequences

- **Facilita:** onboarding do framework, manutenção (1 lugar pra editar), release simplificado
- **Dificulta:** PRs de contribuidores externos têm que editar arquivo grande; risco de conflito em merges
- **Perdemos:** granularidade por país (não dá pra instalar só BR)

## Alternatives considered

- **1 arquivo por país** — manutenção fragmentada, mas PRs isolados
- **Plugin per-country** — over-engineering pro estado atual (VibeDev nem tem plugin marketplace)
- **Status por país (v3.4.0 → v3.4.1)** — classificar como Experimental vs Estável, depois reorientar pra nativo (decisão tomada em v3.4.1)
