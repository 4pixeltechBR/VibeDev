# VibeDev

> Framework de governança para desenvolvimento assistido por IA. Resolve os problemas clássicos de "vibe coding" (dívida técnica, segurança frágil, debug em loop) com disciplina de processo.

## Estrutura

```
vibedev/
├── SKILL.md                              # entry point
├── references/
│   ├── gates.md                          # critérios de transição entre fases
│   ├── handoffs.md                       # ponte para skills-satélite (VibeShield, etc.)
│   ├── stack-guide.md                    # recomendações de ferramentas por caso
│   ├── trilha-verde.md                   # 8 fases para Greenfield (modo técnico)
│   └── trilha-vermelha.md                # 5 fases para Rescue (modo técnico)
└── assets/
    ├── PROJECT_STATE-green.md            # template Trilha Verde (modo técnico)
    ├── PROJECT_STATE-green-leigo.md      # template Trilha Verde (modo leigo) ← NOVO
    ├── PROJECT_STATE-red.md              # template Trilha Vermelha
    └── PROJECT_STATE_ARCHIVE-template.md # esqueleto de arquivo de arquivamento
```

## Como usar

Comando inicial:
```
/vd-start
```

Ciclo:
```
/vd-plan [intenção]   → gera plano, PARA, espera aprovação
/vd-build            → executa o plano aprovado
/vd-check            → valida por evidência (você executa e descreve resultado)
/vd-close            → encerra sessão formalmente
/vd-status           → resumo de 4 linhas
/vd-compact          → arquiva decisões Tipo 2 de fases fechadas
```

## Skills-satélite

VibeDev não faz auditoria de segurança sozinha. Para isso, ela dispara **VibeShield** automaticamente quando gatilhos batem. Veja [references/handoffs.md](./references/handoffs.md) para o protocolo.

```
VibeDev (orquestrador)
    ↓ detecta gatilho G1-G7
VibeShield (auditoria)
    ↓ devolve verdict
VibeDev (integra, segue)
```

## Modo Técnico vs Modo Leigo

A VibeDev detecta automaticamente o perfil do usuário:

| Perfil | Template | Comando |
|---|---|---|
| Dev com formação | `PROJECT_STATE-green.md` | `/vd-start` |
| Pessoa leiga | `PROJECT_STATE-green-leigo.md` | `/vd-start` |

O template leigo tem 5 fases em vez de 8, linguagem natural em vez de jargão, e ativa `modo_usuario: leigo` no estado.

**Importante**: Trilha Vermelha (resgate de projeto existente) **não é recomendada pra leigos**. Apenas devs devem usar.

## Versão

`3.0.0` — adicionou suporte a Modo Leigo e ponte `references/handoffs.md`. Veja [CHANGELOG.md](./CHANGELOG.md).

## Veja também

- [Manual completo](../MANUAL.md)
- [Instalação](../INSTALL.md)
- [VibeShield (auditoria de segurança)](../vibeshield/)