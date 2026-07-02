# Ecossistema VibeDev + VibeShield

> Framework de governança para desenvolvimento assistido por IA. Composto por **VibeDev** (orquestrador de ciclo) e **VibeShield** (auditoria de segurança), com suporte nativo a **Modo Leigo** para usuários sem formação em engenharia.

## O que é isso

Um conjunto de **skills** para assistentes de IA que transformam desenvolvimento "vibe coding" (programação dirigida por IA, sem processo) em desenvolvimento com governança. Funciona em Claude Code, Cursor, Antigravity, OpenCode e Codex.

## Estrutura do ecossistema

| Skill | Papel | Quando entra |
|---|---|---|
| **[VibeDev](./vibedev/)** | Orquestrador do ciclo de desenvolvimento. Define fases, planos, gates, decision log, post-mortems. | Em toda sessão de projeto. |
| **[VibeShield](./vibeshield/)** | Auditora de segurança. Detecta decisões de auth, dados, API, deploy e dispara análise. | Automaticamente via gatilhos G1-G7 quando VibeDev detecta tópicos de risco. |

Skills-satélite adicionais (VibeResearch, VibeDebug, VibeIndex) são previstas no roadmap, mas não estão neste release.

## Dois modos de operação

O ecossistema detecta automaticamente o perfil do usuário através do campo `modo_usuario` em `PROJECT_STATE.md`:

- **`modo_usuario: tecnico`** (padrão): Risk register formal com categorias técnicas C1-C8, gatilhos G#, verdicts `OK/REVISAR/BLOQUEAR`. Pensado para engenheiros.
- **`modo_usuario: leigo`**: Painel visual, perguntas em linguagem natural, 3 opções com recomendação explícita, fallback quando o usuário trava. Pensado para pessoas sem formação técnica.

**Invariante**: o conteúdo escrito em `PROJECT_STATE.md` é sempre técnico. O modo leigo é só renderização. Dev que auditar abre o arquivo e vê o rigor.

## Quem deve usar

- **Dev solo** (engenheiro ou júnior) que quer parar de "Accept All" código de IA sem entender.
- **Indie hacker** que quer lançar produto sem cair em débito técnico de 90 dias.
- **Pessoa leiga** (não-dev) que quer construir um app sem ter que virar dev antes.
- **Time pequeno** que precisa de um "sign-off sênior" automatizado para código AI-assistido (similar à política recente da Amazon).

## Como começar

1. **Instale** a skill no seu ambiente de IA (veja [INSTALL.md](./INSTALL.md)).
2. **Inicie um projeto** com `/vd-start`.
3. **Siga o ciclo**: `/vd-plan` → aprovação → `/vd-build` → `/vd-check`.
4. **VibeShield ativa sozinha** quando você tocar em auth/dados/API/deploy.

Para detalhes, veja o [MANUAL.md](./MANUAL.md).

## Repositório

Skills-versionadas em releases semver:
- VibeDev: `[3.0.0](./vibedev/CHANGELOG.md)` — adicionou suporte a Modo Leigo e ponte `references/handoffs.md`.
- VibeShield: `[1.0.0](./vibeshield/CHANGELOG.md)` — release inicial.

## Licença

MIT. Veja [LICENSE](./LICENSE).

## Aviso

VibeShield é análise por padrões conhecidos. Não substitui pentest humano, scanner real ou revisão manual de AppSec profissional. É um par de olhos adversarial com vocabulário consistente.