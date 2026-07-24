# 0001. VibeDev é uma skill monolítica, não múltiplas skills

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

Frameworks de agent skills têm duas filosofias:
- **Monolítica** — 1 SKILL.md grande, com referências (VibeDev atual)
- **Composta** — N skills pequenas, cada uma com 1 responsabilidade (mattpocock/skills tem 22)

VibeDev atende um público leigo, que precisa de fluxo contínuo e visão integrada do projeto inteiro. Compor 22 skills tornaria o framework opaco pro leigo (qual delas agora? como combinam?).

## Decision

Manter VibeDev como 1 SKILL.md central em `vibedev/SKILL.md`, com 11 comandos e 16 arquivos de referência carregados sob demanda.

## Consequences

- **Facilita:** visão unificada do projeto, fluxo contínuo, posição clara do estado em qualquer momento
- **Dificulta:** plugabilidade (não dá pra instalar só 1 comando), atualizações granulares (mudança em 1 lugar exige release)
- **Perdemos:** instalação modular, descoberta individual por plugins

## Alternatives considered

- **N skills VibeDev (uma por comando)** — quebraria o framework,commands precisariam se coordenar via estado externo
- **Núcleo + satélites (estilo atual com VibeShield)** — já adotado. VibeShield é satélite. Manter.
