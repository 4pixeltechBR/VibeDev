# Instalação — VibeDev

Veja [INSTALL.md principal](../INSTALL.md) para instruções multi-ambiente.

## Instalação rápida (Claude Code)

```bash
mkdir -p ~/.claude/skills
cp -r vibedev ~/.claude/skills/
```

Reinicie o Claude Code.

## Iniciar projeto

```bash
cd /caminho/do/seu/projeto
# Crie PROJECT_STATE.md a partir do template:
cp vibedev/assets/PROJECT_STATE-green.md ./PROJECT_STATE.md
# Ou pra leigos:
cp vibedev/assets/PROJECT_STATE-green-leigo.md ./PROJECT_STATE.md

# Crie o arquivo de detecção do ambiente:
echo "Leia e siga PROJECT_STATE.md na raiz deste projeto antes de qualquer ação. Framework VibeDev ativo." > CLAUDE.md

# Inicie uma sessão com /vd-start
```

## Próximos passos

1. Leia o [MANUAL.md](../MANUAL.md) principal.
2. Comece com `/vd-start`.
3. Para auditoria de segurança, instale também [VibeShield](../vibeshield/).