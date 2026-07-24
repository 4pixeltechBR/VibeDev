# 0006. Model-invoked-anunciado em vez de model-invoked silencioso

- **Status:** Accepted
- **Date:** 2026-07-24
- **Deciders:** Mavis + 4pixeltechBR

## Context

Frameworks como mattpocock/skills dividem skills em user-invoked (humano chama) e model-invoked (modelo chama sozinho). VibeDev atende leigo, que precisa de auditabilidade.

Adotar model-invoked silencioso (modelo age sem avisar) seria o padrão Matt, mas tiraria do leigo a visão do que tá rolando. Tudo user-invoked (status quo) deixaria o leigo perdido entre 11 comandos.

## Decision

Inventamos um meio-termo: **model-invoked-anunciado**. Regras que o modelo aplica sozinho, mas com anúncio visível:

```
IA: "Tô detectando que faz 9 dias que você não mexe. 
Vou aplicar o recap automático. Se quiser pular, fala 
'pula recap'."
```

Aplicado a 4 regras atualmente:
- Glossário Ativo (modo leigo)
- Anti-Feature-Creep
- Recap Automático (7+ dias fora)
- Auto-detecção de Modo País

Comandos terminais (`/vd-kill`, `/vd-launch`, `/vd-plan`, `/vd-help`) continuam user-invoked — decisão humana, não automatizável.

## Consequences

- **Facilita:** automação ganhada + auditabilidade mantida
- **Dificulta:** mais texto na tela (mas o leigo tá acostumado a ler mensagens do framework)
- **Perdemos:** zero fricção (modelo age, leigo nem sabe)

## Alternatives considered

- **Tudo user-invoked** (status quo) — fadiga do leigo ter que lembrar comandos
- **Model-invoked silencioso** (padrão Matt) — opaco pro leigo, vira caixa-preta
- **Model-invoked-anunciado (escolhido)** — automação + auditabilidade

Inspirado por [mattpocock/skills](https://github.com/mattpocock/skills/blob/main/.agents/invocation.md) (referência competitiva, ver `CREDITS.md`).
