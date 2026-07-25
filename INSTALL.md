# Installation / Instalação

> 🇺🇸 English follows; 🇧🇷 Português abaixo.
>
> **Versão:** 3.7.0 (VibeDev v1.8.0 + VibeShield v2.0.0 + 12º comando `/vd-arch-review`)
> **Auditado em:** 2026-07-25
>
> O ecossistema VibeDev tem **3 skills** que podem ser instaladas juntas ou separadas:
> - **VibeDev** (core) — governança de projeto, 12 comandos, modo leigo/técnico
> - **VibeShield** (satélite segurança) — auditoria C1-C8, code-review dedicado
> - **`/vd-arch-review`** (satélite arquitetura) — auditoria de camadas, anti-patterns, débito técnico (faz parte do VibeDev mas pode ser usado standalone)
>
> Para usar **só o VibeDev core**, instale só a pasta `vibedev/`. As outras são opcionais.

---

## 🇺🇸 English

### Which skills to install

| Goal | Install |
|---|---|
| Just project governance (12 commands) | `vibedev/` |
| + security audit (C1-C8) | `vibedev/` + `vibeshield/` |
| + architecture audit (3 levels) | `vibedev/` + `vibeshield/` (arch-review is inside VibeDev) |
| Full ecosystem (recommended) | all three (vibedev + vibeshield) |

All three live in the same `4pixeltechBR/VibeDev` GitHub repo. `vibeshield/` is the only standalone skill; `/vd-arch-review` is a sub-command of VibeDev.

### Installation by environment

#### Claude Code (recommended — full support)

```bash
# Option 1: manual copy (recommended for full control)
mkdir -p ~/.claude/skills
cp -r vibeshield ~/.claude/skills/
cp -r vibedev ~/.claude/skills/

# Option 2: clone the whole repo (recommended for updates)
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

For `references/` and `commands/` (where `/vd-help` and `/vd-arch-review` live), configure in Cursor → Settings → Rules → Custom Rules. Paste the contents of each `*.md` file as a custom rule.

Note: `/vd-arch-review` requires the full file structure (commands/, references/) to work. If only `SKILL.md` is loaded, the command will not be available.

#### Antigravity

```bash
mkdir -p ~/.antigravity/skills
cp -r vibedev ~/.antigravity/skills/
cp -r vibeshield ~/.antigravity/skills/
```

Antigravity loads `SKILL.md` and `references/` natively. Restart Antigravity after copy.

#### OpenCode

```bash
mkdir -p ~/.opencode/skills
cp -r vibedev ~/.opencode/skills/
cp -r vibeshield ~/.opencode/skills/
```

OpenCode loads `SKILL.md` automatically. References may need manual configuration in OpenCode's settings.

#### Codex CLI

```bash
mkdir -p ~/.codex/skills
cp -r vibedev ~/.codex/skills/
cp -r vibeshield ~/.codex/skills/
```

Codex follows the Agent Skills standard. Same structure as Claude Code.

#### Other / Generic

For any environment that supports the Agent Skills standard (`SKILL.md` + `references/` + `assets/`):

```bash
cp -r vibedev <environment-skills-dir>/
cp -r vibeshield <environment-skills-dir>/
```

Check the environment documentation for the correct path.

### Verifying installation

In any new chat, type:

```
Do you have the VibeDev and VibeShield skills loaded? 
List all 12 commands of VibeDev.
```

Expected signs of successful installation (VibeDev + VibeShield):
- Mentions "VibeDev" by name.
- Lists at least these commands: `/vd-spark`, `/vd-start`, `/vd-status`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-kill`, `/vd-close`, `/vd-launch`, `/vd-help`, `/vd-arch-review`.
- Mentions "VibeShield" by name and G1-G7 triggers.
- Mentions Technical vs Layman Mode.
- References `handoff-vibedev.md` or `glossario-leigo.md`.

If it answers generically ("I have some skills"), the skills did not load properly. Troubleshooting below.

### Activating per-project

After installing globally, activate per-project:

1. Navigate to project root.
2. Create `PROJECT_STATE.md` from the appropriate template:
   - **Dev with engineering background:** `vibedev/assets/PROJECT_STATE-green.md` (or `-red.md`).
   - **Layman without engineering background:** `vibedev/assets/PROJECT_STATE-green-leigo.md`.
3. Edit the `Identity` section and save.
4. Create the environment detection file (`CLAUDE.md`, `.cursorrules`, etc.) with the line:

```
Read and follow PROJECT_STATE.md at project root before any action. 
VibeDev framework active.
```

5. Start a session with `/vd-start` (or `/vd-spark` if you only have a vague idea).

### Verifying it works

After `/vd-start` runs, the framework should:
- Detect your `modo_usuario` (leigo or tecnico)
- Show the visual progress panel (leigo) or 4-line status (tecnico)
- Activate Country Mode if you have BR/PT/US/etc signals
- Translate jargon automatically (leigo)

If none of this happens, the `PROJECT_STATE.md` is wrong or the framework didn't load.

### Updating

```bash
cd ~/.claude/skills/VibeDev
git pull origin main
```

Or, if you used manual copy:

```bash
# Re-download and overwrite
rm -rf ~/.claude/skills/vibedev
rm -rf ~/.claude/skills/vibeshield
# then re-copy from the new release zip
```

Check each skill's CHANGELOG before updating projects in progress:
- `vibedev/CHANGELOG.md`
- `vibeshield/CHANGELOG.md`

### Uninstalling

```bash
rm -rf ~/.claude/skills/vibedev
rm -rf ~/.claude/skills/vibeshield
```

Existing `PROJECT_STATE.md` and `CLAUDE.md` files in projects are not modified.

### Troubleshooting

- **Skill not loading:** Confirm `SKILL.md` is at the root of the skill folder. Confirm read permissions. Restart the AI environment completely.
- **Skill loads but does not detect mode:** Open `PROJECT_STATE.md` and confirm `modo_usuario: leigo` (or `tecnico`) field is present in the `Identity` section.
- **VibeShield does not trigger on auth/database/API/deploy:** If manual, trigger with `/vibeshield-audit` (G7).
- **`/vd-arch-review` not available:** Make sure you copied the full `vibedev/` folder including `commands/` subdirectory. The command is in `vibedev/commands/vd-arch-review.md`.
- **Layman gets stuck:** Remind explicitly: "I'm a layman, speak in simple language".
- **Mode País not activating:** Verify your session has BR/PT signals (idioma, moeda, fuso). See `vibedev/references/modo-pais.md` for detection rules.

### Files you'll have after installation

```
~/.claude/skills/
├── vibedev/                    ← core framework
│   ├── SKILL.md                ← entry point (12 comandos documentados)
│   ├── CHANGELOG.md
│   ├── commands/
│   │   ├── vd-help.md          ← router
│   │   └── vd-arch-review.md   ← arquitetura
│   ├── assets/                 ← templates de PROJECT_STATE, ARCH_MAP, IDEA_LOG
│   └── references/             ← 18 references (glossário, modo país, anti-creep, etc)
└── vibeshield/                 ← security satellite
    ├── SKILL.md
    ├── CHANGELOG.md
    ├── commands/
    │   └── code-review.md      ← code-review dedicado
    ├── examples/
    │   └── auth-google.md
    └── references/             ← 4 references (handoff, glossário, formato leigo, estado visual)
```

Then per project:
```
your-project/
├── PROJECT_STATE.md            ← copies from vibedev/assets/PROJECT_STATE-green*.md
├── CLAUDE.md                   ← or .cursorrules, AGENTS.md, etc — env-specific
└── (optional) ARCH_MAP.md      ← generated by /vd-arch-review
```

---

## 🇧🇷 Português

### Quais skills instalar

| Objetivo | Instale |
|---|---|
| Só governança de projeto (12 comandos) | `vibedev/` |
| + auditoria de segurança (C1-C8) | `vibedev/` + `vibeshield/` |
| + auditoria arquitetural (3 níveis) | `vibedev/` + `vibeshield/` (arch-review é parte do VibeDev) |
| Ecossistema completo (recomendado) | os dois |

As duas skills vivem no mesmo repo GitHub `4pixeltechBR/VibeDev`. `vibeshield/` é a única skill standalone; `/vd-arch-review` é sub-comando do VibeDev.

### Instalação por ambiente

#### Claude Code (recomendado — suporte completo)

```bash
# Opção 1: cópia manual (recomendado pra controle total)
mkdir -p ~/.claude/skills
cp -r vibeshield ~/.claude/skills/
cp -r vibedev ~/.claude/skills/

# Opção 2: clonar o repo inteiro (recomendado pra updates)
cd ~/.claude/skills
git clone https://github.com/4pixeltechBR/VibeDev.git
```

Reinicie o Claude Code. As skills carregam na próxima sessão.

#### Cursor

```bash
mkdir -p ~/.cursor/rules
cp vibeshield/SKILL.md ~/.cursor/rules/vibeshield.md
cp vibedev/SKILL.md ~/.cursor/rules/vibedev.md
```

Pra `references/` e `commands/` (onde ficam `/vd-help` e `/vd-arch-review`), configure no Cursor → Settings → Rules → Custom Rules. Cole o conteúdo de cada `*.md` como custom rule.

Atenção: `/vd-arch-review` precisa da estrutura completa (commands/, references/) pra funcionar. Se só o `SKILL.md` for carregado, o comando não vai estar disponível.

#### Antigravity

```bash
mkdir -p ~/.antigravity/skills
cp -r vibedev ~/.antigravity/skills/
cp -r vibeshield ~/.antigravity/skills/
```

Antigravity carrega `SKILL.md` e `references/` nativamente. Reinicie o Antigravity depois da cópia.

#### OpenCode

```bash
mkdir -p ~/.opencode/skills
cp -r vibedev ~/.opencode/skills/
cp -r vibeshield ~/.opencode/skills/
```

OpenCode carrega `SKILL.md` automaticamente. References podem precisar de config manual.

#### Codex CLI

```bash
mkdir -p ~/.codex/skills
cp -r vibedev ~/.codex/skills/
cp -r vibeshield ~/.codex/skills/
```

Codex segue o padrão Agent Skills. Mesma estrutura que Claude Code.

#### Outro / Genérico

Pra qualquer ambiente que suporte o padrão Agent Skills (`SKILL.md` + `references/` + `assets/`):

```bash
cp -r vibedev <diretorio-de-skills-do-ambiente>/
cp -r vibeshield <diretorio-de-skills-do-ambiente>/
```

Consulte a documentação do ambiente pra confirmar o caminho.

### Verificação da instalação

Em qualquer chat novo, digite:

```
Você tem as skills VibeDev e VibeShield carregadas?
Lista os 12 comandos do VibeDev.
```

Sinais de instalação OK (VibeDev + VibeShield):
- Menciona "VibeDev" pelo nome.
- Lista pelo menos: `/vd-spark`, `/vd-start`, `/vd-status`, `/vd-plan`, `/vd-build`, `/vd-check`, `/vd-kill`, `/vd-close`, `/vd-launch`, `/vd-help`, `/vd-arch-review`.
- Menciona "VibeShield" pelo nome e gatilhos G1-G7.
- Menciona Modo Técnico vs Modo Leigo.
- Faz referência ao `handoff-vibedev.md` ou `glossario-leigo.md`.

Se responder genérico ("tenho algumas skills"), as skills não carregaram direito. Troubleshooting abaixo.

### Ativação por projeto

Após instalar globalmente, ative por projeto:

1. Navegue até a raiz do projeto.
2. Crie `PROJECT_STATE.md` a partir do template apropriado:
   - **Dev com formação em engenharia:** `vibedev/assets/PROJECT_STATE-green.md` (ou `-red.md`).
   - **Leigo sem formação em engenharia:** `vibedev/assets/PROJECT_STATE-green-leigo.md`.
3. Edite o `Identidade` e salve.
4. Crie o arquivo de detecção do ambiente (CLAUDE.md, .cursorrules, etc.) com a linha:

```
Leia e siga PROJECT_STATE.md na raiz deste projeto antes de
qualquer ação. Framework VibeDev ativo.
```

5. Inicie uma sessão com `/vd-start` (ou `/vd-spark` se você só tem uma ideia vaga).

### Verificação que funciona

Depois do `/vd-start` rodar, o framework deve:
- Detectar seu `modo_usuario` (leigo ou tecnico)
- Mostrar o painel visual de progresso (leigo) ou status de 4 linhas (tecnico)
- Ativar Modo País se você tem sinais BR/PT/EUA/etc
- Traduzir jargão automaticamente (leigo)

Se nada disso acontecer, o `PROJECT_STATE.md` tá errado ou o framework não carregou.

### Atualização

```bash
cd ~/.claude/skills/VibeDev
git pull origin main
```

Ou, se você usou cópia manual:

```bash
# Re-baixe e sobrescreva
rm -rf ~/.claude/skills/vibedev
rm -rf ~/.claude/skills/vibeshield
# depois copie de novo do zip da release nova
```

Consulte o CHANGELOG de cada skill antes de atualizar projetos em andamento:
- `vibedev/CHANGELOG.md`
- `vibeshield/CHANGELOG.md`

### Desinstalação

```bash
rm -rf ~/.claude/skills/vibedev
rm -rf ~/.claude/skills/vibeshield
```

Os arquivos `PROJECT_STATE.md` e `CLAUDE.md` nos seus projetos não são modificados.

### Solução de problemas

- **A skill não carrega:** Confirme que `SKILL.md` está na raiz da pasta da skill. Confirme permissões de leitura. Reinicie o ambiente de IA completamente.
- **A skill carrega mas não detecta o modo:** Abra `PROJECT_STATE.md` e confirme que o campo `modo_usuario: leigo` (ou `tecnico`) está presente na seção `Identidade`.
- **VibeShield não ativa quando você toca em auth/banco/API/deploy:** Se for manual, dispare com `/vibeshield-audit` (gatilho G7).
- **`/vd-arch-review` não está disponível:** Confirme que copiou a pasta `vibedev/` completa incluindo o subdiretório `commands/`. O comando fica em `vibedev/commands/vd-arch-review.md`.
- **O leigo trava e a IA não muda o tom:** Lembre explicitamente: "Eu sou leigo, fala em linguagem simples".
- **Modo País não ativa:** Verifique se sua sessão tem sinais BR/PT (idioma, moeda, fuso). Veja `vibedev/references/modo-pais.md` pra regras de detecção.

### Arquivos que você terá depois da instalação

```
~/.claude/skills/
├── vibedev/                    ← framework core
│   ├── SKILL.md                ← entry point (12 comandos documentados)
│   ├── CHANGELOG.md
│   ├── commands/
│   │   ├── vd-help.md          ← router
│   │   └── vd-arch-review.md   ← arquitetura
│   ├── assets/                 ← templates de PROJECT_STATE, ARCH_MAP, IDEA_LOG
│   └── references/             ← 18 references (glossário, modo país, anti-creep, etc)
└── vibeshield/                 ← satélite de segurança
    ├── SKILL.md
    ├── CHANGELOG.md
    ├── commands/
    │   └── code-review.md      ← code-review dedicado
    ├── examples/
    │   └── auth-google.md
    └── references/             ← 4 references (handoff, glossário, formato leigo, estado visual)
```

Depois, por projeto:
```
seu-projeto/
├── PROJECT_STATE.md            ← copia de vibedev/assets/PROJECT_STATE-green*.md
├── CLAUDE.md                   ← ou .cursorrules, AGENTS.md, etc — depende do ambiente
└── (opcional) ARCH_MAP.md      ← gerado pelo /vd-arch-review
```

---

## 📚 Documentação adicional no repo

- `README.md` — visão geral bilíngue (PT/EN)
- `MANUAL.md` — guia completo de uso
- `CREDITS.md` — influências (Sandeco Macedo, Matt Pocock)
- `CONTRIBUTING.md` — como contribuir
- `vibedev/CHANGELOG.md` — changelog detalhado do VibeDev
- `vibeshield/CHANGELOG.md` — changelog detalhado do VibeShield
- `.agents/adr/` — 9 Architecture Decision Records
- `.out-of-scope/` (raiz) — 8 docs do que VibeDev NÃO faz
- `vibeshield/.out-of-scope/` — 6 docs do que VibeShield NÃO faz
