# Contributing / Contribuindo

> 🇺🇸 English follows; 🇧🇷 Português abaixo.

---

## 🇺🇸 English

Thanks for wanting to contribute! This ecosystem is deliberately small and opinionated. Good contributions respect the principles.

### Non-negotiable principles

1. **Small skill, strong enforcement.** Don't bloat.
2. **Closed vocabulary.** C1-C8 categories and G1-G7 triggers are closed vocabulary. Proposing additions requires a PR documenting impact.
3. **Layman Mode is rendering only.** Don't duplicate technical rules as layman rules.
4. **Satellite skill has no hands.** Don't add execution.
5. **One persona per skill.** Don't mix roles.

### Opening a PR

1. Fork + branch.
2. Minimal, surgical changes.
3. Update `CHANGELOG.md` of the affected skill (always).
4. If changing categorization or triggers, update `handoff-vibedev.md` + propagate to dependent skill.
5. Use the PR template (`.github/PULL_REQUEST_TEMPLATE.md`).
6. Ensure both modes (Technical + Layman) continue working, if applicable.

### Areas that need help most

- **Empirical testing with real laymen.** We have a hypothesis about how Layman Mode works; only practice confirms.
- **New categories.** Only with usage evidence. C1-C8 covers for now.
- **More complete examples.** Beyond `auth-google.md`.
- **EN/ES translation.** The ecosystem is bilingual by design; English/Spanish documentation is missing.
- **Video tutorials.** Short `/vd-start` tutorials on YouTube.

### Release process

1. Accumulate merged PRs in `main`.
2. When deciding to cut a release: bump semver in CHANGELOG and tag.
3. Semver:
   - **MAJOR** (x.0.0): breaking change in envelopes, categories, or verdicts.
   - **MINOR** (1.x.0): backward-compatible addition (new category, new trigger, new reference).
   - **PATCH** (1.0.x): backward-compatible fix.

---

## 🇧🇷 Português

Obrigado por querer contribuir! Este ecossistema é deliberadamente pequeno e opinativo. Por isso, contribuição boa é contribuição que respeita os princípios.

### Princípios não negociáveis

1. **Pouca skill, forte enforcement.** Não inchar.
2. **Vocabulário fechado.** Categorias C1-C8 e gatilhos G1-G7 são vocabulário fechado. Propor adições exige PR documentando impacto.
3. **Modo Leigo é só renderização.** Não duplicar regras técnicas em regras leigas.
4. **Skill-satélite não tem mãos.** Não adiciona execução.
5. **Persona única por skill.** Não misturar papéis.

### Como abrir PR

1. Fork + branch.
2. Mudanças mínimas e cirúrgicas.
3. Atualizar `CHANGELOG.md` da skill afetada (sempre).
4. Se alterar categorização ou gatilhos, atualizar `handoff-vibedev.md` + propagar pra skill dependente.
5. PR description usando template (`.github/PULL_REQUEST_TEMPLATE.md`).
6. Garantir que ambos os modos (Técnico + Leigo) continuam funcionando, se aplicável.

### Áreas que mais precisam de ajuda

- **Testes empíricos com leigos reais.** Temos hipótese de como o Modo Leigo funciona; só prática confirma.
- **Categorias novas.** Só com evidência de uso. Por enquanto C1-C8 cobre.
- **Exemplos completos.** Mais cenários além de `auth-google.md`.
- **Tradução EN/ES.** O ecossistema é bilíngue no design; documentação em inglês e espanhol estão em falta.
- **Documentação em vídeo.** Tutoriais curtos de `/vd-start` no YouTube.

### Processo de release

1. Acumular PRs merged em `main`.
2. Quando decidir cortar release: bump semver em CHANGELOG e tag.
3. Semver:
   - **MAJOR** (x.0.0): breaking change em envelopes, categorias ou veredictos.
   - **MINOR** (1.x.0): adição retrocompatível (nova categoria, novo gatilho, novo reference).
   - **PATCH** (1.0.x): correção retrocompatível.