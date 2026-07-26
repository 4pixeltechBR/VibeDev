# v3.7.0 — Pacotes de instalação DEV

> **Idiomas:** 🇺🇸 English follows; 🇧🇷 Português abaixo.

## 🇺🇸 English

This directory contains **pre-built zips for direct installation** of VibeDev v3.7.0 (VibeDev v1.8.0 + VibeShield v2.0.0 + 12th command `/vd-arch-review`) into your development environment, without needing to clone or copy the whole repo.

### Which zip to use

| Zip | Size | When to use |
|---|---|---|
| **`VibeDev-v3.7.0-DEV-CLAUDE-CODE.zip`** | 144 KB | Claude Code (full support — all 12 commands work) |
| **`VibeDev-v3.7.0-DEV-CURSOR.zip`** | 28 KB | Cursor (limited — only `SKILL.md` files, custom rules for the rest) |
| **`VibeDev-v3.7.0-DEV-MULTI.zip`** | 152 KB | Antigravity, OpenCode, Codex, or any Agent-Skills-standard environment |

### Quick install (Claude Code)

```bash
unzip VibeDev-v3.7.0-DEV-CLAUDE-CODE.zip

mkdir -p ~/.claude/skills
cp -r claude-code/vibedev ~/.claude/skills/
cp -r claude-code/vibeshield ~/.claude/skills/

# Restart Claude Code
```

Verify in any new chat:
```
Do you have the VibeDev and VibeShield skills loaded?
List all 12 commands of VibeDev.
```

### Quick install (Antigravity / OpenCode / Codex)

```bash
unzip VibeDev-v3.7.0-DEV-MULTI.zip

# Pick your environment:
mkdir -p ~/.antigravity/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.antigravity/skills/

# OR
mkdir -p ~/.opencode/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.opencode/skills/

# OR
mkdir -p ~/.codex/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.codex/skills/
```

### Quick install (Cursor)

```bash
unzip VibeDev-v3.7.0-DEV-CURSOR.zip

mkdir -p ~/.cursor/rules
cp cursor/vibedev.md ~/.cursor/rules/vibedev.md
cp cursor/vibeshield.md ~/.cursor/rules/vibeshield.md

# For /vd-arch-review and other commands, paste the contents of
# vibedev/commands/*.md as Custom Rules in Cursor → Settings → Rules
```

### What each zip contains

```
DEV-CLAUDE-CODE.zip:
├── INSTALL.md          (full installation guide, bilingual)
├── vibedev/            (complete core framework)
│   ├── SKILL.md
│   ├── commands/       (vd-help.md, vd-arch-review.md)
│   ├── assets/         (PROJECT_STATE templates, ARCH_MAP template, IDEA_LOG template)
│   └── references/     (18 references: glossary, country mode, anti-creep, etc)
└── vibeshield/         (complete security satellite)
    ├── SKILL.md
    ├── commands/       (code-review.md)
    ├── examples/       (auth-google.md)
    └── references/     (4 references)

DEV-CURSOR.zip:
├── INSTALL.md
├── README.md
├── vibedev.md          (the SKILL.md content for Cursor rules)
└── vibeshield.md       (the SKILL.md content for Cursor rules)

DEV-MULTI.zip:
├── INSTALL.md
├── README.md
├── vibedev/            (complete, same as Claude Code zip)
└── vibeshield/         (complete, same as Claude Code zip)
```

### After install: per-project setup

1. Go to project root
2. Copy `PROJECT_STATE.md` template:
   - Layman: `vibedev/assets/PROJECT_STATE-green-leigo.md` → `./PROJECT_STATE.md`
   - Dev: `vibedev/assets/PROJECT_STATE-green.md` (or `-red.md`) → `./PROJECT_STATE.md`
3. Edit the `Identity` section
4. Create environment detection file:
   - Claude Code: `CLAUDE.md` with the line "Read and follow PROJECT_STATE.md at project root before any action. VibeDev framework active."
   - Cursor: custom rule with the same instruction
   - Antigravity: `AGENTS.md`
5. Start with `/vd-start` (or `/vd-spark` if you have a vague idea)

### Why not just clone the repo?

Cloning is also valid:
```bash
cd ~/.claude/skills
git clone https://github.com/4pixeltechBR/VibeDev.git
```

**Pros of zips:** smaller download, no `.git/` history, no `.github/`, `.changeset/`, etc. — just the skills you need.
**Pros of git clone:** easy updates with `git pull`, full history, can switch between versions.

Pick what fits your workflow.

### Updating

For zips: download the new version, replace the old folder.
For git clone: `cd ~/.claude/skills/VibeDev && git pull origin main`.

Check `vibedev/CHANGELOG.md` and `vibeshield/CHANGELOG.md` before updating.

---

## 🇧🇷 Português

Este diretório contém **zips pré-montados pra instalação direta** do VibeDev v3.7.0 (VibeDev v1.8.0 + VibeShield v2.0.0 + 12º comando `/vd-arch-review`) no seu ambiente de desenvolvimento, sem precisar clonar ou copiar o repo inteiro.

### Qual zip usar

| Zip | Tamanho | Quando usar |
|---|---|---|
| **`VibeDev-v3.7.0-DEV-CLAUDE-CODE.zip`** | 144 KB | Claude Code (suporte completo — todos os 12 comandos funcionam) |
| **`VibeDev-v3.7.0-DEV-CURSOR.zip`** | 28 KB | Cursor (limitado — só `SKILL.md`, custom rules pro resto) |
| **`VibeDev-v3.7.0-DEV-MULTI.zip`** | 152 KB | Antigravity, OpenCode, Codex, ou qualquer ambiente padrão Agent Skills |

### Install rápido (Claude Code)

```bash
unzip VibeDev-v3.7.0-DEV-CLAUDE-CODE.zip

mkdir -p ~/.claude/skills
cp -r claude-code/vibedev ~/.claude/skills/
cp -r claude-code/vibeshield ~/.claude/skills/

# Reinicie o Claude Code
```

Verifique em qualquer chat novo:
```
Você tem as skills VibeDev e VibeShield carregadas?
Lista os 12 comandos do VibeDev.
```

### Install rápido (Antigravity / OpenCode / Codex)

```bash
unzip VibeDev-v3.7.0-DEV-MULTI.zip

# Escolha seu ambiente:
mkdir -p ~/.antigravity/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.antigravity/skills/

# OU
mkdir -p ~/.opencode/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.opencode/skills/

# OU
mkdir -p ~/.codex/skills
cp -r multi-harness/vibedev multi-harness/vibeshield ~/.codex/skills/
```

### Install rápido (Cursor)

```bash
unzip VibeDev-v3.7.0-DEV-CURSOR.zip

mkdir -p ~/.cursor/rules
cp cursor/vibedev.md ~/.cursor/rules/vibedev.md
cp cursor/vibeshield.md ~/.cursor/rules/vibeshield.md

# Pra /vd-arch-review e outros commands, cole o conteúdo de
# vibedev/commands/*.md como Custom Rules no Cursor → Settings → Rules
```

### O que cada zip contém

```
DEV-CLAUDE-CODE.zip:
├── INSTALL.md          (guia completo de instalação, bilíngue)
├── vibedev/            (framework core completo)
│   ├── SKILL.md
│   ├── commands/       (vd-help.md, vd-arch-review.md)
│   ├── assets/         (templates PROJECT_STATE, ARCH_MAP, IDEA_LOG)
│   └── references/     (18 references: glossário, modo país, anti-creep, etc)
└── vibeshield/         (satélite de segurança completo)
    ├── SKILL.md
    ├── commands/       (code-review.md)
    ├── examples/       (auth-google.md)
    └── references/     (4 references)

DEV-CURSOR.zip:
├── INSTALL.md
├── README.md
├── vibedev.md          (conteúdo do SKILL.md pra Cursor rules)
└── vibeshield.md       (conteúdo do SKILL.md pra Cursor rules)

DEV-MULTI.zip:
├── INSTALL.md
├── README.md
├── vibedev/            (completo, igual ao zip do Claude Code)
└── vibeshield/         (completo, igual ao zip do Claude Code)
```

### Depois do install: setup por projeto

1. Vá até a raiz do projeto
2. Copie template de `PROJECT_STATE.md`:
   - Leigo: `vibedev/assets/PROJECT_STATE-green-leigo.md` → `./PROJECT_STATE.md`
   - Dev: `vibedev/assets/PROJECT_STATE-green.md` (ou `-red.md`) → `./PROJECT_STATE.md`
3. Edite a seção `Identidade`
4. Crie arquivo de detecção do ambiente:
   - Claude Code: `CLAUDE.md` com a linha "Leia e siga PROJECT_STATE.md na raiz deste projeto antes de qualquer ação. Framework VibeDev ativo."
   - Cursor: custom rule com a mesma instrução
   - Antigravity: `AGENTS.md`
5. Comece com `/vd-start` (ou `/vd-spark` se você tem só uma ideia vaga)

### Por que não só clonar o repo?

Clonar também é válido:
```bash
cd ~/.claude/skills
git clone https://github.com/4pixeltechBR/VibeDev.git
```

**Prós dos zips:** download menor, sem `.git/`, sem `.github/`, `.changeset/` etc — só as skills.
**Prós do git clone:** updates fáceis com `git pull`, histórico completo, pode trocar entre versões.

Escolha o que cabe no seu workflow.

### Atualização

Pros zips: baixe a versão nova, substitua a pasta antiga.
Pro git clone: `cd ~/.claude/skills/VibeDev && git pull origin main`.

Consulte `vibedev/CHANGELOG.md` e `vibeshield/CHANGELOG.md` antes de atualizar.

---

## 🔗 Ver também

- [INSTALL.md](../../INSTALL.md) — guia completo bilíngue
- [README.md](../../README.md) — visão geral do projeto
- [v3.7.0 release notes](../../docs/releases/v3.7.0.md) — detalhes da release
