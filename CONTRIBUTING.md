# Contribuindo

Obrigado por querer contribuir! Este ecossistema é deliberadamente pequeno e opinativo. Por isso, contribuição boa é contribuição que respeita os princípios.

## Princípios não negociáveis

1. **Pouca skill, forte enforcement.** Não inchar.
2. **Vocabulário fechado.** Categorias C1-C8 e gatilhos G1-G7 são vocabulário fechado. Propor adições exige PR documentando impacto.
3. **Modo Leigo é só renderização.** Não duplicar regras técnicas em regras leigas.
4. **Skill-satélite não tem mãos.** Não adiciona execução.
5. **Persona única por skill.** Não misturar papéis.

## Como abrir PR

1. Fork + branch.
2. Mudanças mínimas e cirúrgicas.
3. Atualizar `CHANGELOG.md` da skill afetada (sempre).
4. Se alterar categorização ou gatilhos, atualizar `handoff-vibedev.md` + propagar pra skill dependente.
5. PR description usando template (`.github/PULL_REQUEST_TEMPLATE.md`).
6. Garantir que ambos os modos (Técnico + Leigo) continuam funcionando, se aplicável.

## Áreas que mais precisam de ajuda

- **Testes empíricos com leigos reais.** Temos hipótese de como o Modo Leigo funciona; só prática confirma.
- **Categorias novas.** Só com evidência de uso. Por enquanto C1-C8 cobre.
- **Exemplos completos.** Mais cenários além de `auth-google.md`.
- **Tradução EN/ES.** O ecossistema é bilíngue no design; documentação em inglês e espanhol estão em falta.
- **Documentação em vídeo.** Tutoriais curtos de `/vd-start` no YouTube.

## Código de conduta

Respeito mútuo. Crítica técnica é boa; desqualificação pessoal não. Decisões são técnicas, não pessoas.

## Reportar bug

Use `.github/ISSUE_TEMPLATE/bug_report.md`. Preencha o ambiente, modo, passos pra reproduzir.

## Sugerir feature

Use `.github/ISSUE_TEMPLATE/feature_request.md`. Antes de sugerir, confira se já não foi rejeitada em issue existente — muitas sugestões já foram pensadas e decididas contra por princípio.

## Processo de release

1. Acumular PRs merged em `main`.
2. Quando decidir cortar release: bump semver em CHANGELOG e tag.
3. Semver:
   - **MAJOR** (x.0.0): breaking change em envelopes, categorias ou veredictos.
   - **MINOR** (1.x.0): adição retrocompatível (nova categoria, novo gatilho, novo reference).
   - **PATCH** (1.0.x): correção retrocompatível.

## Mantenedores

Por enquanto, manutenção é feita por contribuidores iniciais. Conforme crescer, adicionamos CODEOWNERS.