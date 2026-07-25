# `.arch-review/` Template

> Cada arquivo aqui é gerado/atualizado por `/vd-arch-review` quando o
> nível correspondente roda. Conteúdo de exemplo abaixo — apague e
> deixe o framework regenerar.

---

## `SUMMARY.md` (índice executivo)

```markdown
# Resumo de Auditoria Arquitetural

**Projeto:** [nome]
**Última auditoria:** AAAA-MM-DD HH:MM
**Comando usado:** /vd-arch-full | /vd-arch-only | /vd-arch-health
**Veredito:** OK | REVISAR | BLOQUEAR

## Achados por nível

### Nível 1 — Macro
- [N] achados (raro, geralmente listagem só)

### Nível 2 — Meso
- [N] achados

### Nível 3 — Micro
- [N] achados (🔴 R / 🟡 A / 🔵 N)

## Top 3 ações

1. [título + caminho + esforço]
2. [...]
3. [...]

Próximo passo: [ver ARCH_MAP.md na raiz]
```

---

## `macro-stack.md` (Nível 1)

Conteúdo gerado por `references/arch-review-macro.md`.

---

## `meso-arch.md` (Nível 2)

Conteúdo gerado por `references/arch-review-meso.md`.
Inclui diagrama de componentes Mermaid + fluxos críticos.

---

## `micro-health.md` (Nível 3)

Conteúdo gerado por `references/arch-review-micro.md`.
Inclui lista priorizada de débitos com severidade e esforço.
