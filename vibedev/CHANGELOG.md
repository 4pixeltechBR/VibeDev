# Changelog — VibeDev

Todas as mudanças notáveis neste projeto são documentadas aqui.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [3.0.0] — 2026-07-02

### Adicionado
- `references/handoffs.md` — ponte de despacho para skills-satélite. Define o protocolo de detecção de modo (`modo_usuario`), lista de skills-satélite com formato compacto (quem/quando/o que/como integrar) e referências cruzadas para `references/handoff-vibedev.md` da VibeShield (canônico).
- `assets/PROJECT_STATE-green-leigo.md` — template de estado em Modo Leigo. Tem 5 fases em vez de 8, descrição de cada fase em linguagem natural, sem jargão de engenharia, foco em `modo_usuario: leigo` desde o nascimento do projeto.
- Documentação atualizada em `SKILL.md` (comando `/vd-start`):
  - Tabela de decisão de template por perfil de usuário.
  - Como perguntar em linguagem natural se o usuário tem formação.
  - Adaptação do conteúdo de `trilha-verde.md` para linguagem leiga quando o template leigo for escolhido.

### Modificado
- `SKILL.md` (`/vd-start`): agora detecta perfil do usuário e seleciona template apropriado entre `PROJECT_STATE-green.md` (técnico) ou `PROJECT_STATE-green-leigo.md` (leigo). Antes, só existia o template técnico.

### Sem quebra de compatibilidade
- Versões anteriores de projetos com `PROJECT_STATE.md` no template técnico continuam funcionando.
- O campo `modo_usuario` é opt-in. Se ausente, VibeShield assume Modo Técnico (fallback seguro).
- VibeShield como satélite continua opcional — projetos que não usam VibeShield não são afetados.

## [1.0.0] — 2025-XX-XX

### Adicionado
- Release inicial da VibeDev como framework de governança para desenvolvimento assistido por IA.
- Comandos: `/vd-start`, `/vd-status`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-close`, `/vd-compact`.
- Templates de estado: `PROJECT_STATE-green.md`, `PROJECT_STATE-red.md`, `PROJECT_STATE_ARCHIVE-template.md`.
- Referências: `gates.md`, `stack-guide.md`, `trilha-verde.md`, `trilha-vermelha.md`.
- Persona: Arquiteto-Guardião.
- Princípio central: estado em arquivo, nunca na memória da conversa.

[3.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v3.0.0
[1.0.0]: https://github.com/4pixeltechBR/VibeDev/releases/tag/v1.0.0