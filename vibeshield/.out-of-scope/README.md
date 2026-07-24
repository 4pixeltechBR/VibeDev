# Out of Scope — VibeShield

> Documenta coisas que VibeShield **deliberadamente não faz** e por quê.
> Espelha o padrão de [VibeDev/.out-of-scope](../../.out-of-scope/) e de
> [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/.out-of-scope) (referência competitiva).

## Como usar este diretório

Antes de abrir issue ou PR pedindo uma feature nova, verifique aqui. Se a feature está listada, a issue provavelmente será fechada com link pra cá.

## Conteúdo

| Arquivo | O que NÃO fazemos |
|---|---|
| `not-a-pentest.md` | VibeShield audita código, não faz penetration test |
| `not-replacing-dpo-lawyer.md` | LGPD/GDPR/PIPL etc precisam de counsel humano, não ferramenta |
| `not-a-sast-tool.md` | Não substitui ferramentas como Semgrep/Snyk/CodeQL |
| `not-covering-runtime-race-conditions.md` | Auditoria estática, não dinâmica |
| `not-audit-transitive-deps.md` | Só o que tá escrito no projeto, não a árvore completa |
| `not-replacing-vibedev.md` | VibeShield é satélite, não substitui governança de projeto |
