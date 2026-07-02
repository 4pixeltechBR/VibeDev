# Installation / Instalação

> 🇺🇸 English follows; 🇧🇷 Português abaixo.

---

## 🇺🇸 English

### Installation by environment

#### Claude Code

```bash
# Option 1: manual copy
mkdir -p ~/.claude/skills
cp -r vibeshield ~/.claude/skills/
cp -r vibedev ~/.claude/skills/

# Option 2: clone directly
cd ~/.claude/skills
git clone https://github.com/4pixeltechBR/VibeDev.git
```

Restart Claude Code. Skills load automatically on the next session.

#### Cursor

```bash
mkdir -p ~/.cursor/rules
cp vibeshield/SKILL.md ~/.cursor/rules/vibeshield.md
cp vibedev/SKILL.md ~/.cursor/rules/vibedev.md
```

For `references/`, configure in Cursor → Settings → Rules → Custom Rules.

#### Antigravity / OpenCode / Codex

See specific environment documentation. Generally, copy `SKILL.md` and the `references/` and `assets/` folders to the environment's skill location.

### Verifying installation

In any new chat, type:

```
Do you have the VibeShield skill loaded? Tell me what you can do 
with it.
```

Expected signs of successful installation:
- Mentions "VibeShield" by name.
- Lists G1-G7 triggers or part of them.
- Mentions Technical vs Layman Mode.
- References `handoff-vibedev.md` or `glossario-leigo.md`.

If it answers generically ("I have security tools"), the skill did not load properly.

### Activating per-project

After installing globally, activate per-project:

1. Navigate to project root.
2. Create `PROJECT_STATE.md` from the appropriate template:
   - **Dev** with engineering background: `vibedev/assets/PROJECT_STATE-green.md` (or `-red.md`).
   - **Layman** without engineering background: `vibedev/assets/PROJECT_STATE-green-leigo.md`.
3. Edit the `Identity` section and save.
4. Also create the environment detection file (`CLAUDE.md`, `.cursorrules`, etc.) with the line:

```
Read and follow PROJECT_STATE.md at project root before any action. 
VibeDev framework active.
```

5. Start a session with `/vd-start`.

### Updating

```bash
cd ~/.claude/skills/VibeDev
git pull origin main
```

Check each skill's CHANGELOG before updating projects in progress.

### Uninstalling

```bash
rm -rf ~/.claude/skills/vibeshield
rm -rf ~/.claude/skills/vibedev
```

Existing `PROJECT_STATE.md` and `CLAUDE.md` files in projects are not modified.

### Troubleshooting

- **Skill not loading**: Confirm `SKILL.md` is at the root of the skill folder. Confirm read permissions. Restart the AI environment completely.
- **Skill loads but does not detect mode**: Open `PROJECT_STATE.md` and confirm `modo_usuario: leigo` (or `tecnico`) field is present in the `Identity` section.
- **VibeShield does not trigger on auth/database/API/deploy**: If manual, trigger with `/vibeshield-audit` (G7).
- **Layman gets stuck**: Remind explicitly: "I'm a layman, speak in simple language".

---

## 🇧🇷 Português

### Instalação por ambiente

#### Claude Code

```bash
# Opção 1: cópia manual
mkdir -p ~/.claude/skills
cp -r vibeshield ~/.claude/skills/
cp -r vibedev ~/.claude/skills/

# Opção 2: clone direto
cd ~/.claude/skills
git clone https://github.com/4pixeltechBR/VibeDev.git
```

Reinicie o Claude Code. As skills carregam automaticamente na próxima sessão.

#### Cursor

```bash
mkdir -p ~/.cursor/rules
cp vibeshield/SKILL.md ~/.cursor/rules/vibeshield.md
cp vibedev/SKILL.md ~/.cursor/rules/vibedev.md
```

Para as `references/`, configure no Cursor → Settings → Rules → Custom Rules.

#### Antigravity / OpenCode / Codex

Veja documentação específica do ambiente. Em geral, copie `SKILL.md` e as pastas `references/` e `assets/` para o local de skills do ambiente.

### Verificação da instalação

Em qualquer chat novo, digite:

```
Você tem a skill VibeShield carregada? Me diz o que você consegue 
fazer com ela.
```

Resposta esperada (sinais de instalação OK):
- Menciona "VibeShield" pelo nome.
- Lista gatilhos G1-G7 ou parte deles.
- Menciona Modo Técnico vs Modo Leigo.
- Faz referência ao `handoff-vibedev.md` ou `glossario-leigo.md`.

Se responder de forma genérica ("tenho ferramentas de segurança"), a skill não carregou direito.

### Ativação por projeto

Após instalar globalmente, ative por projeto:

1. Navegue até a raiz do projeto.
2. Crie `PROJECT_STATE.md` a partir do template apropriado:
   - **Dev** com formação em engenharia: `vibedev/assets/PROJECT_STATE-green.md` (ou `-red.md`).
   - **Leigo** sem formação em engenharia: `vibedev/assets/PROJECT_STATE-green-leigo.md`.
3. Edite o `Identidade` e salve.
4. Crie também o arquivo de detecção do ambiente (CLAUDE.md, .cursorrules, etc.) com a linha:

```
Leia e siga PROJECT_STATE.md na raiz deste projeto antes de 
qualquer ação. Framework VibeDev ativo.
```

5. Inicie uma sessão com `/vd-start`.

### Atualização

```bash
cd ~/.claude/skills/VibeDev
git pull origin main
```

Consulte o CHANGELOG de cada skill para ver o que mudou antes de atualizar projetos em andamento.

### Desinstalação

```bash
rm -rf ~/.claude/skills/vibeshield
rm -rf ~/.claude/skills/vibedev
```

Os arquivos `PROJECT_STATE.md` e `CLAUDE.md` nos seus projetos não serão alterados.

### Solução de problemas

- **A skill não carrega**: Confirme que `SKILL.md` está na raiz da pasta da skill. Confirme permissões de leitura. Reinicie o ambiente de IA completamente.
- **A skill carrega mas não detecta o modo**: Abra `PROJECT_STATE.md` e confirme que o campo `modo_usuario: leigo` (ou `tecnico`) está presente na seção `Identidade`.
- **VibeShield não ativa quando você toca em auth/banco/API/deploy**: Se for manual, dispare com `/vibeshield-audit` (gatilho G7).
- **O leigo trava e a IA não muda o tom**: Lembre explicitamente: "Eu sou leigo, fala em linguagem simples".