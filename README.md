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
- **`modo_usuario: leigo`** (Layman Mode): Visual progress panel, non-technical natural language questions, 3 recommended options with justifications, and safe fallbacks.

*Note: The written state in `PROJECT_STATE.md` always remains technical. Layman Mode only simplifies the conversational rendering.*

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
- **`modo_usuario: leigo`**: Painel visual de progresso, perguntas em linguagem simples, 3 opções recomendadas com justificativa e caminhos padrão seguros.

*Nota: O estado persistido em `PROJECT_STATE.md` continua técnico. O Modo Leigo altera apenas a renderização conversacional.*

### 🛠️ Como Começar

1. Instale as skills no seu ambiente de IA (veja [INSTALL.md](./INSTALL.md)).
2. Inicie o projeto com `/vd-start`.
3. Siga o ciclo: `/vd-plan` ➔ aprovação ➔ `/vd-build` ➔ `/vd-check`.
4. A VibeShield é ativada sozinha quando código sensível for detectado.

---

## 📄 License & Terms / Licença & Termos

MIT License. See [LICENSE](./LICENSE) for details. / Licença MIT. Detalhes em [LICENSE](./LICENSE).
