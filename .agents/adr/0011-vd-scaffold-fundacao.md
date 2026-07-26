# 0011. v3.9 — `/vd-scaffold` como sub-comando de fundação técnica

- **Status:** Accepted
- **Date:** 2026-07-26
- **Deciders:** Mavis + 4pixeltechBR
- **Inspired by:** Dante Testa — Prompt Mestre WordPress v1.14.0 (jul/2026)

## Context

A v3.8.0 introduziu orquestração em camadas (autoria, advogado do diabo,
MANUAL.md). Identificamos que, durante a Fase 3 — Arquitetura da Trilha Verde,
o `/vd-build` ainda é genérico: pede sub-tarefas ao usuário, codifica, mas não
tem **protocolo de fundação** dedicado.

Problemas concretos observados:

1. **Stack fixada por hábito:** devs (inclusive agentes) escolhem a stack
   que conhecem (Node+Express, Django, Laravel) sem perguntar ao usuário.
   Decisão Tipo 1 tomada por default.
2. **Schema sem migrations versionadas:** código com `CREATE TABLE` solto,
   sem up/down reversível. Drift entre dev/staging/prod inevitável.
3. **RBAC genérico:** "tem admin e usuário" como resposta. Sem matriz
   Role × Recurso × Ação × Escopo. Implementação hardcoded em 5 if/else.
4. **Pagamento USA-first por default:** Stripe como primeira opção. Quem
   vende no Brasil descobre que precisa de PIX e que webhook idempotente
   é obrigatório (não nice-to-have).
5. **White-label inexistente:** cores hardcoded em CSS, logo hardcoded em
   `<img>`. Multi-tenant é promessa, não realidade.
6. **Coexistência com legado:** WordPress, ERP, planilha — agente não sabe
   como abordar. Escolhe uma das 3 opções (geralmente a pior).

Produtos pagos (Dante Testa) já resolveram isso de forma fechada
(WordPress-only, R$ 50, opaco). VibeDev, sendo horizontal e open source,
precisa resolver de forma stack-agnostic e auditável.

## Decision

Adicionar **1 sub-comando** + **6 references** que juntos formam o "motor
de fundação técnica" do VibeDev, ativado automaticamente como gate no
`/vd-build`.

### Estrutura

```
vibedev/
├── commands/
│   └── vd-scaffold.md              ← entry point
└── references/
    ├── scaffold-stack-validada.md       ← 3 stacks + caso de borda
    ├── scaffold-schema-migrations.md    ← schema agnóstico
    ├── scaffold-rbac.md                 ← matriz 4D
    ├── scaffold-pagamento-br.md         ← PIX + MP + Asaas + Stripe
    ├── scaffold-white-label.md          ← tokens parametrizados
    └── scaffold-legado-coexistencia.md  ← WordPress/ERP/planilha
```

### Gate de ativação (no `/vd-build`)

Dispara automaticamente quando:
- `trilha: verde` em `PROJECT_STATE.md`
- Fase atual = **3 (Arquitetura)**
- Sub-tarefa envolve qualquer um destes:
  - Modelagem de dados
  - Autenticação / permissões
  - Pagamentos
  - Multi-tenancy
  - White-label
  - Estrutura inicial de pastas

Opt-out via flag `--skip-scaffold`.

### Protocolo (em ordem)

1. **Pré-check (v3.8):** se `Stack decidida` em PROJECT_STATE está preenchida
   mas triagem sugere outra, dispara `/vd-devils-advocate` como contradição.
2. **Ler estado** do `PROJECT_STATE.md`.
3. **Triagem Cynefin-lite:** 3 perguntas máximo, mapeia pra 3 perfis.
4. **Red Team:** "o que quebra essa stack em 12 meses?"
5. **Gerar 4 componentes** via references (schema, RBAC, pagamento, white-label).
6. **Registrar ADR** no Decision Log.
7. **Devolver ao VibeDev:** marca sub-tarefa pronta pra `/vd-check`.

### Decisões de design críticas

| Decisão | Escolha | Razão |
|---------|---------|-------|
| Skill vs sub-comando | **Sub-comando** | Manter "família de 3 skills"; trigger é fase-específico, não standalone |
| 3 stacks pré-validadas | SaaS / MVP / (legado = caso de borda) | Cynefin em forma de tabela; WordPress/ERP = Tipo 1 sempre |
| Stack "SaaS real" | FastAPI + React + TS + Postgres | Padrão VibeDev (local-first, contratos Pydantic, sem lock-in) |
| Stack "MVP" | Next.js + SQLite (migra pra Postgres) | Time-to-mercado; Drizzle é portável |
| Pagamento primário | PIX + MP + Asaas (3 providers) | BR-first; redundância operacional |
| Pagamento fallback | Stripe | Internacional; explicitamente fallback, não primário |
| Webhook idempotência | UNIQUE em `(provider, event_id)` | Obrigatório; sem isso = cobrança duplicada |
| RBAC implementação | 3 opções (coluna, tabela, Casbin) | Agnóstico; tamanho do projeto decide |
| White-label | Tokens via CSS vars ou Tailwind | Agnóstico de framework; 3 opções |
| Legado/WordPress | Caso de borda, 3 opções | VibeDev é horizontal, não vira framework WP |
| Versão semântica | Skill 1.x → **2.0.0** | Mudança significativa (novo gate, 7 arquivos novos) |

## Consequences

### Easier
- Decisão de stack vira consciente (Cynefin + Red Team + Advogado do Diabo).
- Schema gerado é copy-paste funcional (migrations reversíveis, índices,
  multi-tenancy desde o dia 1).
- Pagamento BR-first pronto (PIX, MP, Asaas) com idempotência, não retrofit.
- RBAC real, não "admin e usuário".
- White-label parametrizado desde o commit 1, não retrofit caro.
- Coexistência com legado deixa de ser mistério (3 opções, sempre).
- Integra com v3.8 (Advogado do Diabo dispara em tensão stack).
- Métrica `stack_trocada` dá feedback sobre qualidade da Triagem.

### Harder
- `/vd-build` na Fase 3 fica mais lento (gate adiciona ~3-5 min em sub-tarefas
  de fundação).
- Curva de aprendizado: 6 references + 1 command file = 57 KB de doc.
- Usuários apressados vão usar `--skip-scaffold` em tudo (anti-padrão).
- White-label gate H2 sugerido mas não implementado em v3.9 — vai pra v4.0.
- Métrica `stack_trocada` é auto-avaliação (sujeita a esquecimento).

### Given up
- Sem scaffold para Node.js puro (só FastAPI Python e Next.js TS como
  pré-validados). Outros stacks vão pra Tipo 1 de alta incerteza.
- Sem geração automática de diagramas (ex: ER do schema). Vai pra backlog.
- Sem suporte a MongoDB/Cassandra/etc — Postgres é a única opção não-MVP.
  Tradeoff explícito: simplicidade > cobertura exaustiva.
- Sem integração nativa com providers de email/SMS (SendGrid, Twilio) — só
  pagamento. Outros serviços vão pra Tipo 1.

## Alternatives considered

### A) vd-scaffold como 4ª skill (junto com VibeDev, VibeShield, vd-arch-review)
Rejeitado. Dispara a mesma skill várias vezes se Fase 3 tem múltiplas
sub-tarefas de fundação. Skill é cara; sub-comando é barato. Mantém
"família de 3 skills" enxuta.

### B) Implementar dentro de `/vd-build` direto (sem sub-comando)
Rejeitado. SKILL.md ficaria 4x maior. 6 references não teriam como existir
sem sub-comando. Opt-out (`--skip-scaffold`) seria difícil.

### C) Apenas 1 reference "tudo sobre scaffold" (sem 6 separadas)
Rejeitado. Cada componente (schema, RBAC, pagamento, white-label) é
grande demais pra 1 arquivo. Misturar tudo vira novelão ilegível.

### D) Stack única (forçar FastAPI + React sempre)
Rejeitado. Quebra horizontalidade do VibeDev. MPV com SQLite é caso
comum e legítimo; forçar Postgres desde o dia 1 atrasa validação.

### E) WordPress como stack pré-validada
Rejeitado. VibeDev é horizontal; virar "framework WordPress" é anti-
histórico. WordPress/ERP vira caso de borda, sempre Tipo 1 de alta
incerteza, com 3 opções apresentadas.

### F) Lançar vd-scaffold como produto separado (`4pixeltechBR/vd-scaffold`)
Rejeitado. Cria overhead de manutenção de 2 repos. Acopla releases.
Comando vive melhor dentro de VibeDev, como sub-comando.

## Implementation

### Commits
- `2fa6baa` — feat(v3.9): vd-scaffold motor de fundacao tecnica (7 arquivos
  novos, 1 modificado)

### Arquivos criados (7)
1. `vibedev/commands/vd-scaffold.md` (7.3 KB)
2. `vibedev/references/scaffold-stack-validada.md` (6.2 KB)
3. `vibedev/references/scaffold-schema-migrations.md` (7.0 KB)
4. `vibedev/references/scaffold-rbac.md` (6.6 KB)
5. `vibedev/references/scaffold-pagamento-br.md` (11.1 KB)
6. `vibedev/references/scaffold-white-label.md` (10.2 KB)
7. `vibedev/references/scaffold-legado-coexistencia.md` (9.0 KB)

Total: ~57 KB de doc + 1 commit.

### Modified (2)
- `vibedev/SKILL.md` (gate no `/vd-build`, lista de 13 comandos, tabela de invocação)
- `vibedev/CHANGELOG.md` (entrada `[2.0.0]`)

## References

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026) — gerador de
  fundação com pagamento próprio e webhook idempotente
- **Cynefin Framework** (Dave Snowden, 1999) — base da Triagem
- **Strangler Fig Pattern** (Martin Fowler, 2004) — coexistência com legado
- **Change Data Capture** (Debezium, 2015+) — sincronização bidirecional
- **PCI-DSS 4.0** (2024) — segurança de pagamento
- **LGPD Art. 46** (Brasil, 2020) — privacidade em pagamento
- **CSS Custom Properties** — base de white-label agnóstico
- **CREDITS.md** — Sandeco Macedo, Matt Pocock
- ADR 0006 (model-invoked-anunciado) — gate é anunciado, não silencioso
- ADR 0008 (vd-arch-review) — pattern de "skill-satélite-sub-comando"

## What's next

- **v4.0 (major):** Gate H2 do VibeShield (heurística de design tokens) implementado.
- **v4.0 (major):** `vd-status` v2 com mapa visual incluindo scaffold.
- **v4.x:** Scaffold para Node.js (Express + TS + Postgres) e Go (Gin + Postgres).
- **v4.x:** Diagrama ER automático a partir de schema.
