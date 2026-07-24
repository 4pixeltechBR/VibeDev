# 0005. Citação formal a Sandeco Macedo via CREDITS.md

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

VibeDev v3.1.0+ foi influenciado pelo paper de Macedo (arXiv:2607.00038, junho 2026). Reconhecimento estava em release notes + comentários. Decidimos criar arquivo permanente de créditos.

## Decision

Arquivo `CREDITS.md` na raiz do repo, com seções:
- Papers & academic work (Macedo)
- Open-source projects we learn from
- Tooling & infrastructure
- Community contributors (vai crescendo)

Adicionalmente, abrir issue pública em `sandeco/reversa/issues/21` com reconhecimento (v3.4.2).

## Consequences

- **Facilita:** rastreabilidade de influências, posição clara de proveniência, contribuidores externos se sentem reconhecidos
- **Dificulta:** manutenção (entrada nova exige PR, precisa atualização de versões)
- **Perdemos:** tempo de manutenção (aceitável, vale história)

## Alternatives considered

- **README tem créditos** — espalha, polui o README
- **Só no release notes** — histórico fragmenta, fácil perder
- **Menção em CONTRIBUTING.md** — sem destaque visual suficiente
