# 0007. VibeShield code-review como skill dedicada, não comando na SKILL principal

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

VibeShield tinha 1 SKILL.md com audit gatilho (G1-G7) e 1 categoria C1-C8. Adicionar `/vibeshield-code-review` poderia ser:
- (A) Comando na SKILL.md principal (junto com audit gatilho)
- (B) Skill dedicada em `commands/code-review.md`

Frameworks como mattpocock/skills têm N skills pequenas, cada uma com 1 responsabilidade. VibeShield tem 1 skill monolítica (por design, ADR 0001).

## Decision

Skill dedicada em `vibeshield/commands/code-review.md`. User-invoked, sob demanda, 2 eixos (Standards + Spec). SKILL.md principal preservada focada em audit gatilho (G1-G7).

## Consequences

- **Facilita:** separação clara (audit vs review), user pode chamar uma sem a outra, code-review pode ser expandida sem tocar na skill principal
- **Dificulta:** adicionar 1 arquivo novo em vez de seção nova na SKILL.md
- **Perdemos:** proximidade textual entre os dois conceitos

## Alternatives considered

- **Comando na SKILL.md principal** — escolhido, mas rejeitado: SKILL.md principal é sobre audit gatilho. Misturar os dois diluiria foco.
- **Skill totalmente separada (outro SKILL.md próprio)** — rejeitado: é satélite, deve viver na pasta `vibeshield/`.
- **Fusão com `/vd-check` do VibeDev** — rejeitado: VibeDev checa **se funcionou**, VibeShield audita **se é seguro**. São eixos diferentes.

## Invariantes

- `commands/code-review.md` referencia o handoff canônico (`references/handoff-vibedev.md`) pra envelope de escrita no estado
- Audit gatilho (G1-G7) e code-review são **complementares**, não substitutos
- C1-C8 não se aplicam a code-review (que opera em outro eixo: Standards + Spec)
- Versão VibeShield bumpada pra 2.0.0 por mudança de escopo (não quebra de comportamento)
