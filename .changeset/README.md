# Changesets

> Cada PR que muda comportamento deve adicionar um arquivo `.md` aqui descrevendo
> o que mudou. No release, os changesets viram entrada no `CHANGELOG.md` automaticamente.

Inspirado por [mattpocock/skills](https://github.com/mattpocock/skills) (referência competitiva) e [Changesets](https://github.com/changesets/changesets).

## Como adicionar um changeset

1. Crie um arquivo `.changeset/<nome-descritivo>.md`
2. Use o template abaixo
3. Abra PR com esse arquivo
4. No release, os changesets viram commit + tag + CHANGELOG entry automaticamente

## Template

```markdown
---
"vibedev-ecosystem": minor
---

Descrição em 1-3 frases do que mudou. Linguagem humana, não changelog bot.

Pra breaking changes, use `major`. Pra novas features, `minor`. Pra fix, `patch`.
```

## Tipos de bump

| Tipo | Quando usar | Exemplo |
|---|---|---|
| `major` (x.0.0) | Quebra comportamento existente | Remoção de comando, mudança em trigger |
| `minor` (1.x.0) | Adição retrocompatível | Novo comando, novo reference, novo out-of-scope |
| `patch` (1.0.x) | Correção retrocompatível | Fix typo, ajuste de texto, hyperlink |

## Estado atual

Esta pasta é **read by humans** até ativar workflow de bot. Por enquanto, mudanças ainda são descritas manualmente nos CHANGELOGs.

## Workflow de release (futuro)

1. Acumular changesets em `.changeset/*.md` durante PRs
2. Bot (ou maintainer) roda `pnpm changeset version`
3. Changesets viram entrada em `CHANGELOG.md` + `vibedev/CHANGELOG.md` + `vibeshield/CHANGELOG.md`
4. Bot faz commit + tag
5. Maintainer roda release no GitHub
