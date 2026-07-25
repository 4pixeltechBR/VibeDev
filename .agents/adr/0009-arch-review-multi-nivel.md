# 0009. `/vd-arch-review` opera em 3 níveis: macro (stack), meso (arquitetura), micro (saúde)

- **Status:** Accepted
- **Date:** 2026-07-25
- **Deciders:** Mavis + 4pixeltechBR

## Context

Auditoria arquitetural pode ter profundidades diferentes. Opções:
- 1 nível (tudo de uma vez) — simples mas lento, consome tokens
- 2 níveis (rápido + completo) — escolha binária
- 3 níveis (macro/meso/micro) — progressão natural, escolha granular
- N níveis (4+) — over-engineering pra público leigo

Sandeco Macedo's paper (arXiv:2607.00038) define "5-level verification ladder" como conceito. Aplicar análogo aqui: cada nível = profundidade diferente, com escopo claro.

## Decision

3 níveis, mapeados pra 3 sub-comandos:

| Nível | Nome | Comando | Escopo | Saída |
|---|---|---|---|---|
| 1 | Macro (Stack/Topologia) | (parte de `/vd-arch-full`) | Linguagens, frameworks, infra, contêineres, deps, estrutura de pastas | Inventário + diagrama de containers |
| 2 | Meso (Arquitetura/Fluxo) | `/vd-arch-only` | Módulos, bounded contexts, modelo de dados, rotas de API, fluxos críticos | Diagrama de componentes Mermaid + diagrama de sequência |
| 3 | Micro (Saúde/Débito) | `/vd-arch-health` | Hotspots, acoplamento, vazamento de abstração, dep vs custom, cobertura de testes, anti-patterns | Lista priorizada de débitos + esboço de refactor |

**Mapeamento comandos:**
- `/vd-arch-full` = nível 1 + 2 + 3 (completo, mais lento)
- `/vd-arch-only` = nível 2 (arquitetura + componentes)
- `/vd-arch-health` = nível 3 (saúde + débito)

## Consequences

- **Facilita:** escolha granular, cada nível é útil sozinho, vocabulário de Sandeco aplicado de forma análoga
- **Dificulta:** usuário precisa saber qual nível quer (mas só 3, não 5+)
- **Perdemos:** simplicidade de 1 comando único

## Alternatives considered

- **1 nível** (tudo de uma vez) — rejeitado: dev que só quer débito não precisa esperar 5min por stack analysis
- **2 níveis** (rápido + completo) — rejeitado: granularidade insuficiente
- **5 níveis** (Sandeco literal) — rejeitado: público leigo da VibeDev não quer 5 níveis. Adaptação analógica, não literal.

## Invariantes

- Cada nível tem escopo auto-contido
- `/vd-arch-full` é a soma dos 3, não análise diferente
- Diagrama Mermaid sai em todos os níveis (não só meso)
- Estado em `ARCH_MAP.md` é **incremental**: rodar nível 2 depois do 1 atualiza, não recria
