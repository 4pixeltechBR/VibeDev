# Changelog — VibeShield

Todas as mudanças notáveis neste projeto são documentadas aqui.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [1.0.0] — 2026-06-30

### Adicionado
- `SKILL.md` — entry point com princípios não negociáveis, gatilhos G1-G7, 3 verdicts canônicos, formato de envelope.
- `references/handoff-vibedev.md` — contrato canônico com VibeDev. Inclui:
  - Vocabulário fechado (sub-tarefa ativa, decisão Tipo 1/Tipo 2, gatilho G#, etc.).
  - Protocolo de disparo VibeDev → satélite.
  - Protocolo de execução da skill-satélite.
  - Formato de envelope para saídas.
  - As 8 categorias canônicas (C1-C8) com mapeamento OWASP Top 10.
  - 3 verdicts canônicos (OK / REVISAR / BLOQUEAR).
  - Regras de acesso ao estado (só-acrescentar, timestamp ISO).
  - 5 situações de erro (E1-E5).
  - **Modo Leigo** (seção 0): detecção automática via `modo_usuario` no estado, fallback seguro.
- `references/glossario-leigo.md` — dicionário de tradução C1-C8 → pergunta em linguagem natural, com 3 opções recomendadas por categoria e frases-âncora.
- `references/formato-conversa-leigo.md` — 3 templates de saída conversacional (Decisão Necessária, Aviso Rápido, Conversa de Ajuda) + regras de abertura, encerramento, fallback, regra dos 3 minutos.
- `references/estado-visual.md` — camada visual que renderiza o `PROJECT_STATE.md` em painel resumido pro leigo.
- `examples/auth-google.md` — walkthrough end-to-end de Login com Google OAuth mostrando os dois modos (Técnico + Leigo) em paralelo. Stack exemplo: Node.js / Express.

### Princípios estabelecidos

1. Skill-satélite não tem mãos (só análise).
2. Skill-satélite não decide pelo usuário.
3. Categorias C1-C8 são vocabulário fechado.
4. Verdict BLOQUEAR é bloqueio real, não conselho.
5. Estado compartilhado via `PROJECT_STATE.md`, modo só-acrescentar.

[1.0.0]: https://github.com/seu-usuario/vibeshield/releases/tag/v1.0.0