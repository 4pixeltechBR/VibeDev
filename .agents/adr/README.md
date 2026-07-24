# Architecture Decision Records (ADRs)

> Histórico de decisões arquiteturais importantes do VibeDev.
> Cada decisão vira um arquivo. Decisões são **datadas, contextualizadas, e imutáveis** — se substituídas, linka pra nova decisão.

## Como escrever um ADR novo

1. Copie `template.md` → `NNNN-titulo-curto.md` (número sequencial)
2. Preencha as seções: Contexto, Decisão, Consequências, Alternativas consideradas
3. Commit como docs/adr separadamente da implementação, ou junto
4. Se a decisão for substituída: marque `Status: Superseded by NNNN` e link

## Status possíveis

- **Proposed** — em discussão
- **Accepted** — decidido, ativo
- **Superseded by NNNN** — substituído por ADR mais novo (linka pro novo)
- **Deprecated** — não-aplicado mais, sem substituto

## Por que ADRs importam

Decisões como "por que VibeDev é monolítica" ou "por que Modo País tem 12 países" são invisíveis no commit history. ADRs as tornam explícitas e recuperáveis.

Inspirado em [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/.agents/adr) (referência competitiva, ver `CREDITS.md`).
