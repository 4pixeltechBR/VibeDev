# Handoffs — VibeDev ↔ Skills-Satélite

> Este documento lista as skills-satélite que podem ser disparadas durante o ciclo VibeDev. Cada entrada é uma linha de instrução: **QUANDO** chamar, **O QUE** delegar, **COMO** integrar de volta.

**Princípio único**: a VibeDev continua sendo o sistema nervoso central. Skills-satélite só entram em pontos específicos do ciclo e devolvem resultado pra `PROJECT_STATE.md`.

---

## VibeShield — Segurança

**Quem**: skill `vibeshield`. Auditora de segurança (Modo Técnico / Modo Leigo).

**Quando chamar** — um dos 7 gatilhos bate:

- **G1**: sub-tarefa toca **autenticação** (login, sessão, JWT, OAuth, MFA, etc.)
- **G2**: sub-tarefa toca **persistência de dados** (banco, schema, migrações, ORM)
- **G3**: sub-tarefa toca **integração com API de terceiro** (Stripe, SendGrid, OpenAI, etc.)
- **G4**: sub-tarefa toca **exposição pública** (deploy, domínio, env vars de produção)
- **G5**: dependência nova entra no projeto (`package.json`, `requirements.txt`, etc.)
- **G6**: sub-tarefa reprovou 2x no `/vd-check` (modo DEBUG)
- **G7**: usuário pediu explicitamente

**Lista de palavras-gatilho** completa está no handoff canônico, não duplicar aqui.

**O que delegar**: análise das 8 categorias (C1-C8) + 3 verdicts (OK / REVISAR / BLOQUEAR).

**Como integrar de volta**:

- **`OK`** → anexar saída do `vibeshield:plan-check` (ou outro comando) como evidência do critério de "pronto" da sub-tarefa, no `PROJECT_STATE.md`.
- **`REVISAR`** → anexar ao Decision Log; mencionar nas 3 opções do `/vd-plan`.
- **`BLOQUEAR`** → bloquear `/vd-build` até o usuário fechar a decisão Tipo 1.

**Detecção do modo de comunicação**: ler `modo_usuario` no `PROJECT_STATE.md`. `leigo` → Modo Leigo (ver `references/glossario-leigo.md`, `references/formato-conversa-leigo.md`, `references/estado-visual.md` da VibeShield). Ausente ou `tecnico` → Modo Técnico (ver `references/handoff-vibedev.md` da VibeShield).

**Onde está a verdade**: `references/handoff-vibedev.md` da skill `vibeshield` é o **contrato canônico**. Este arquivo é só uma ponte de despacho rápida. Em caso de divergência, o handoff da VibeShield vence.

---

## Próximas skills (placeholder)

Quando outras skills-satélite forem adicionadas ao ecossistema, a entrada dela entra aqui com o mesmo formato: **quem / quando / o que / como integrar**.

Até lá, **não invente** skills-satélite na hora. Toda nova skill precisa de:
1. Contrato próprio parecido com `references/handoff-vibedev.md` da VibeShield.
2. Item listado aqui com o mesmo formato compacto.
3. Conformidade declarada com versão do handoff.

---

**Versão**: 1.0.0 (esqueleto)
