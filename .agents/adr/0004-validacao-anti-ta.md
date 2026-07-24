# 0004. Validação anti-"tá" no /vd-check

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

Leigos tendem a dizer "tá", "deve tá", "acho que sim" pra encerrar `/vd-check` rápido. Aceitar como aprovação = feature não testada vai pra produção.

## Decision

`/vd-check` reformula pergunta vaga em evento observável específico. Se usuário responde vago de novo, entra em modo DEBUG pedindo print, log, ou observação.

## Consequences

- **Facilita:** confiança no estado, menos "tá" como falso positivo, leigo aprende a testar
- **Dificulta:** fricção adicional pro leigo apressado (1-2 reformulações)
- **Perdemos:** velocidade de fechamento de sub-tarefa (aceitável, vale integridade)

## Alternatives considered

- **Aceitar "tá" como aprovação** — atual antes de v3.4.0. Rejeitado: acumula bugs.
- **Forçar sempre print** — quebraria em ambiente sem screenshot
- **Auto-testar via comando** — possível, mas tira autonomia do humano
