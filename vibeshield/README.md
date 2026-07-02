# VibeShield

> Skill-satélite de auditoria de segurança para o ciclo VibeDev.

VibeShield é chamada automaticamente pela VibeDev quando uma sub-tarefa toca em tópicos sensíveis (autenticação, banco, integração com API, deploy). Ela detecta as 8 categorias de risco (C1-C8), categoriza achados em verdicts `OK` / `REVISAR` / `BLOQUEAR`, e devolve o resultado no `PROJECT_STATE.md`.

## Estrutura

```
vibeshield/
├── SKILL.md                        # entry point
├── examples/
│   └── auth-google.md              # exemplo end-to-end (Modo Técnico + Leigo)
└── references/
    ├── handoff-vibedev.md          # contrato canônico com VibeDev
    ├── glossario-leigo.md          # tradução C1-C8 → linguagem natural
    ├── formato-conversa-leigo.md   # 3 templates de saída conversacional
    └── estado-visual.md            # painel visual de progresso
```

## Como usar

Você normalmente não chama a VibeShield diretamente. Ela entra automaticamente quando o gatilho bate (veja `references/handoff-vibedev.md`).

Mas se quiser disparar manual:
```
/vibeshield-audit [coisa]
```

## Modo Técnico vs Modo Leigo

- **`modo_usuario: tecnico`** (padrão): risk register formal com C1-C8, gatilhos G#, verdicts.
- **`modo_usuario: leigo`**: painel visual, perguntas em linguagem natural, 3 opções com recomendação.

## Limites

- **Não tem mãos.** Analisa, categoriza, recomenda. Não roda scanner, não edita código, não modifica estado fora da sub-tarefa ativa.
- **Não decide pelo usuário.** Devolve opções; VibeDev transforma em Tipo 1; usuário aprova.
- **Não substitui pentest humano.** É análise por padrões conhecidos.

## Versão

`1.0.0` — release inicial. Veja [CHANGELOG.md](./CHANGELOG.md).

## Veja também

- [Manual completo](../MANUAL.md)
- [Instalação](../INSTALL.md)
- [VibeDev (orquestrador)](../vibedev/)