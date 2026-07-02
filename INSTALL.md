# Manual de Instalação

## Instalação por ambiente

### Claude Code

```bash
# Opção 1: cópia manual
mkdir -p ~/.claude/skills
cp -r vibeshield ~/.claude/skills/
cp -r vibedev ~/.claude/skills/

# Opção 2: clone direto
cd ~/.claude/skills
git clone https://github.com/seu-usuario/vibeshield.git
git clone https://github.com/seu-usuario/vibedev.git
```

Reinicie o Claude Code. As skills carregam automaticamente na próxima sessão.

### Cursor

```bash
mkdir -p ~/.cursor/rules
cp vibeshield/SKILL.md ~/.cursor/rules/vibeshield.md
cp vibedev/SKILL.md ~/.cursor/rules/vibedev.md
```

Para as `references/`, configure no Cursor → Settings → Rules → Custom Rules.

### Antigravity / OpenCode / Codex

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

## Ativação por projeto

Após instalar globalmente, ative por projeto:

1. Navegue até a raiz do projeto.
2. Crie `PROJECT_STATE.md` a partir do template apropriado:
   - **Dev** com formação: `vibedev/assets/PROJECT_STATE-green.md` (ou `-red.md`).
   - **Leigo** sem formação: `vibedev/assets/PROJECT_STATE-green-leigo.md`.
3. Edite o `Identidade` e salve.
4. Crie também o arquivo de detecção do ambiente (CLAUDE.md, .cursorrules, etc.) com a linha:

```
Leia e siga PROJECT_STATE.md na raiz deste projeto antes de 
qualquer ação. Framework VibeDev ativo.
```

5. Inicie uma sessão com `/vd-start`.

## Atualização

Para atualizar para uma versão mais recente:

```bash
cd ~/.claude/skills/vibeshield  # ou vibedev
git pull origin main
```

Consulte o CHANGELOG de cada skill para ver o que mudou antes de atualizar projetos em andamento.

## Desinstalação

```bash
rm -rf ~/.claude/skills/vibeshield
rm -rf ~/.claude/skills/vibedev
```

Os arquivos `PROJECT_STATE.md` e `CLAUDE.md` nos seus projetos não serão alterados. Eles continuam existindo mas sem o framework por trás.

## Solução de problemas

### A skill não carrega
- Confirme que `SKILL.md` está na raiz da pasta da skill.
- Confirme permissões de leitura.
- Reinicie o ambiente de IA completamente.

### A skill carrega mas não detecta o modo
- Abra `PROJECT_STATE.md` e confirme que o campo `modo_usuario: leigo` (ou `tecnico`) está presente na seção `Identidade`.
- Peça pra IA reler o estado explicitamente: "Por favor, releia o PROJECT_STATE.md e me diz o que está nele."

### VibeShield não ativa quando você toca em auth/banco/API/deploy
- Verifique se VibeDev está em ciclo ativo (sub-tarefa aberta).
- Se for manual, dispare com `/vibeshield-audit` (gatilho G7).

### Como o leigo trava e a IA não muda o tom
- Lembre explicitamente: "Eu sou leigo, fala em linguagem simples".
- Se ainda assim vier jargão, é bug — reporte.

## Suporte

Issues no GitHub do repositório. Não há suporte pago.