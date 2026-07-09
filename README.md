# 🚀 VibeDev + VibeShield Ecosystem / Ecossistema VibeDev + VibeShield

🌍 **Read in / Leia em:** [English](#-english) | [Português](#-português)

---

## 🇺🇸 English

[![VibeDev License](https://img.shields.io/github/license/4pixeltechBR/VibeDev?style=flat-square&color=blue)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/4pixeltechBR/VibeDev?style=flat-square)](https://github.com/4pixeltechBR/VibeDev/stargazers)
[![Framework Version](https://img.shields.io/badge/version-3.0.0-emerald?style=flat-square)](vibedev/SKILL.md)

**VibeDev** is a lightweight, rigorous governance framework specifically designed for AI-driven software development (Vibe Coding). Coupled with **VibeShield**, it provides automated security audits and native **Layman Mode** support.

### 🗺️ Ecosystem Structure

| Component | Role | When it triggers |
|---|---|---|
| **[VibeDev](./vibedev/)** | Development cycle orchestrator. Defines phases, plans, gates, and post-mortems. | Every project session. |
| **[VibeShield](./vibeshield/)** | Security auditor. Detects authentication, database, API, and deploy actions. | Automatically via triggers G1-G7. |

### 🧭 Two Modes of Operation

The ecosystem automatically detects user profile via the `modo_usuario` field in `PROJECT_STATE.md`:
- **`modo_usuario: tecnico`** (default): Formal risk registers, C1-C8 vulnerability categories, and OK/REVIEW/BLOCK verdicts.
- **`modo_usuario: leigo`** (Layman Mode): Auto-translated jargon (Glossary), mandatory cost estimation (Cost Guard), and `/vd-kill` for graceful exit.

*Note: The written state in `PROJECT_STATE.md` always remains technical. Layman Mode only simplifies the conversational rendering.*

### 🆕 What's new in v3.1.0

- 📖 **Glossário Ativo** — auto-translates jargon in Layman Mode (see `vibedev/references/glossario-leigo.md`).
- 💰 **Guarda de Custo** — mandatory 3-tier monthly cost estimation before leaving Phase 3 (see `vibedev/references/estimativa-custos.md`).
- 🪦 **`/vd-kill`** — graceful project termination command, archives state to `IDEA_LOG.md` instead of deleting. Anti-guilt.

### 🆕 What's new in v3.2.0

- ⏱️ **Estimativas de Tempo Calibradas** — realistic construction time by project category, surfaced before `/vd-start` (see `vibedev/references/tempo-construcao.md`).
- 🛡️ **Anti-Feature-Creep** — 3-layer scope containment during `/vd-build`. Ideas land in backlog, not in code (see `vibedev/references/anti-creep.md`).
- 🎯 **Reconhecimento & Pausa** — embedded rituals for sober progress acknowledgment + pause prompts at phase gates. Anti-burnout, anti-guilt.

### 🆕 What's new in v3.3.0 (Layman Lifecycle Complete)

- ✨ **`/vd-spark`** — pre-Phase-1 discovery for laymen with vague ideas. 4-round conversation extracts idea/persona/transformation/success-signal before `/vd-start` runs. See `vibedev/references/discovery-leigo.md`.
- 🚀 **`/vd-launch`** — post-Phase-7 communication blocks (elevator pitch, 3 tweet variants, LinkedIn post, landing page structure, beta email, private post-mortem) + 24h pre-launch checklist in `vibedev/assets/launch/`.
- 🇧🇷 **Modo Brasil** — auto-activated when pt-BR / BR context detected. BRL, Pix-first, LGPD explicit, BR hosting options. See `vibedev/references/brasil.md`.

### 🛠️ How to Start

1. Install the skills in your AI environment (see [INSTALL.md](./INSTALL.md)).
2. Initialize your project with `/vd-start`.
3. Follow the cycle: `/vd-plan` ➔ approval ➔ `/vd-build` ➔ `/vd-check`.
4. VibeShield will trigger automatically when security-sensitive code is touched.

---

## 🇧🇷 Português

[![VibeDev License](https://img.shields.io/github/license/4pixeltechBR/VibeDev?style=flat-square&color=blue)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/4pixeltechBR/VibeDev?style=flat-square)](https://github.com/4pixeltechBR/VibeDev/stargazers)
[![Versão do Framework](https://img.shields.io/badge/vers%C3%A3o-3.0.0-emerald?style=flat-square)](vibedev/SKILL.md)

O **VibeDev** é um framework leve e rigoroso de governança para desenvolvimento assistido por IA (Vibe Coding). Integrado ao **VibeShield**, oferece auditorias automáticas de segurança e suporte nativo a **Modo Leigo**.

### 🗺️ Estrutura do Ecossistema

| Componente | Papel | Quando entra |
|---|---|---|
| **[VibeDev](./vibedev/)** | Orquestrador de ciclo de desenvolvimento. Define fases, planos, gates e post-mortems. | Em toda sessão do projeto. |
| **[VibeShield](./vibeshield/)** | Auditor de segurança. Detecta ações de autenticação, banco de dados, API e deploy. | Automaticamente via gatilhos G1-G7. |

### 🧭 Dois Modos de Operação

O ecossistema detecta automaticamente o perfil do usuário através do campo `modo_usuario` em `PROJECT_STATE.md`:
- **`modo_usuario: tecnico`** (padrão): Registro de riscos formal, categorias de vulnerabilidade C1-C8 e vereditos OK/REVISAR/BLOQUEAR.
- **`modo_usuario: leigo`**: Jargão traduzido automaticamente (Glossário), estimativa de custo obrigatória (Guarda de Custo), e `/vd-kill` pra encerramento digno.

*Nota: O estado persistido em `PROJECT_STATE.md` continua técnico. O Modo Leigo altera apenas a renderização conversacional.*

### 🆕 O que tem de novo no v3.1.0

- 📖 **Glossário Ativo** — traduz jargão automaticamente no Modo Leigo (ver `vibedev/references/glossario-leigo.md`).
- 💰 **Guarda de Custo** — estimativa de custo mensal obrigatório em 3 portes antes de sair da Fase 3 (ver `vibedev/references/estimativa-custos.md`).
- 🪦 **`/vd-kill`** — comando de encerramento digno do projeto, arquiva o estado em `IDEA_LOG.md` em vez de apagar. Anti-culpa.

### 🆕 O que tem de novo no v3.2.0

- ⏱️ **Estimativas de Tempo Calibradas** — tempo realista de construção por categoria de projeto, apresentado antes do `/vd-start` (ver `vibedev/references/tempo-construcao.md`).
- 🛡️ **Anti-Feature-Creep** — contenção de escopo em 3 camadas durante `/vd-build`. Ideias vão pro backlog, não pro código (ver `vibedev/references/anti-creep.md`).
- 🎯 **Reconhecimento & Pausa** — rituais embutidos de reconhecimento sóbrio de progresso + prompts de pausa em gates de fase. Anti-burnout, anti-culpa.

### 🆕 O que tem de novo no v3.3.0 (Ciclo Leigo Completo)

- ✨ **`/vd-spark`** — discovery pré-Fase-1 pra leigos com ideia vaga. Conversa de 4 rodadas extrai ideia/persona/transformação/sinal-de-sucesso antes do `/vd-start` rodar (ver `vibedev/references/discovery-leigo.md`).
- 🚀 **`/vd-launch`** — blocos de comunicação pós-Fase-7 (elevator pitch, 3 variantes de tweet, post LinkedIn, estrutura de landing, e-mail beta, post-mortem privado) + checklist 24h antes do lançamento em `vibedev/assets/launch/`.
- 🇧🇷 **Modo Brasil** — auto-ativado quando detecta pt-BR / contexto BR. BRL, Pix-first, LGPD explícita, hospedagem BR. Ver `vibedev/references/brasil.md`.

### 🛠️ Como Começar

1. Instale as skills no seu ambiente de IA (veja [INSTALL.md](./INSTALL.md)).
2. Inicie o projeto com `/vd-start`.
3. Siga o ciclo: `/vd-plan` ➔ aprovação ➔ `/vd-build` ➔ `/vd-check`.
4. A VibeShield é ativada sozinha quando código sensível for detectado.

---

## 📄 License & Terms / Licença & Termos

MIT License. See [LICENSE](./LICENSE) for details. / Licença MIT. Detalhes em [LICENSE](./LICENSE).
