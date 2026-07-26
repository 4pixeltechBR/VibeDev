# /vd-scaffold

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Motor de Fundação Técnica do VibeDev. Roda **dentro** de `/vd-build` quando
> a sub-tarefa envolve fundação (banco, auth, pagamento, multi-tenant, white-label)
> na **Fase 3 — Arquitetura** da Trilha Verde.

## 🇧🇷 O que faz

Transforma a intenção já aprovada em `/vd-plan` em **fundação técnica real**:
schema + migrations, matriz RBAC, integração de pagamento BR-first com webhook
idempotente, e tokens de white-label. **Não fixa stack** — decide por contexto
via Triagem Cynefin-lite.

**Princípio central:** stack é decisão Tipo 1. Nunca escolhe por hábito nem
aceita primeira opção sem Red Team. O código gerado tem que ser **copy-paste
funcional** — declarar "estrutura pronta pra escala" sem entregar migration
e índice reais é proibido aqui.

## 🇺🇸 What it does

Turns the intent already approved in `/vd-plan` into **real technical foundation**:
schema + migrations, RBAC matrix, BR-first payment integration with idempotent
webhook, and white-label tokens. **Does not fix stack** — decides by context
via Cynefin-lite triage.

**Central principle:** stack is Type 1 decision. Never chooses by habit nor
accepts the first option without Red Team. Generated code must be **copy-paste
functional** — declaring "structure ready to scale" without delivering real
migration and index is forbidden here.

---

## 🚦 Quando ativar (gate)

**Ativação automática** quando TODAS as condições:

- `trilha: verde` em `PROJECT_STATE.md`
- Fase atual = **Fase 3 — Arquitetura** (ou o usuário marcar explicitamente "fase de fundação")
- Sub-tarefa aprovada em `/vd-plan` envolve QUALQUER destes:
  - Modelagem de dados (banco, schema)
  - Autenticação / permissões
  - Pagamentos / assinaturas
  - Multi-tenancy
  - White-label / identidade visual parametrizada
  - Estrutura inicial de pastas do projeto

**Ativação explícita** quando o usuário digitar `/vd-scaffold` em qualquer momento da Fase 3.

**NÃO ativar** se:
- `PROJECT_STATE.md` não existe → redirecionar para `/vd-start` primeiro
- Trilha ≠ Verde → redirecionar para Trilha Vermelha (`references/trilha-vermelha.md`)
- Sub-tarefa é claramente Tipo 2 (script isolado, ajuste pontual) → `/vd-build` direto

---

## 📋 Protocolo (nesta ordem)

### 0. Pré-check (v3.8+: gate com Advogado do Diabo)

Antes de tudo, leia `PROJECT_STATE.md` e verifique:

| Estado | Ação |
|--------|------|
| `Stack decidida` **vazia** | Rode Triagem (passo 2) |
| `Stack decidida` **preenchida** | Confirme a stack; pule Triagem |
| `Stack decidida` preenchida MAS triagem sugere outra | **Dispare `/vd-devils-advocate`** (gate v3.8) com a tensão como contradição interna |

Se a Triagem ainda não foi rodada, registre a data da triagem e a stack escolhida
na seção `Stack decidida` do `PROJECT_STATE.md`.

### 1. Ler estado

Leia `PROJECT_STATE.md`. Confirme: trilha = Verde, fase = 3 (Arquitetura),
sub-tarefa atual envolve fundação.

### 2. Triagem de stack (Cynefin-lite)

**Máximo 3 perguntas, nunca mais:**

1. "Este projeto é SaaS multi-cliente, ferramenta interna de uso único, ou
   MVP pra validar antes de investir?"
2. "Escala esperada em 12 meses: dezenas de usuários, milhares, ou você ainda
   não sabe?"
3. "Existe algum sistema legado (WordPress, ERP, planilha) que precisa
   continuar vivo junto com isso?"

Mapeie para uma das 3 stacks validadas (ver `references/scaffold-stack-validada.md`):

| Perfil | Stack | Quando faz sentido |
|--------|-------|-------------------|
| SaaS real / multi-cliente, escala incerta pra cima | FastAPI + React + TS + Postgres (+Redis se fila/cache justificar) | Padrão consistente com projetos local-first — contratos Pydantic, sem vendor lock-in |
| MVP de validação rápida | Next.js fullstack + SQLite (migra pra Postgres se validar) | Time-to-mercado importa mais que escala ainda não comprovada |
| Caso de borda: sistema legado coexistindo | Tratar como **Tipo 1 de alta incerteza** | Migrar sairia mais caro que integrar. WordPress/ERP/etc = caso especial, ver `references/scaffold-legado-coexistencia.md` |

Se a resposta não se encaixar limpo em nenhuma linha, trate como **Tipo 1 de
alta incerteza**: apresente as opções com prós/contras/custo oculto e pergunte —
não decida sozinho.

### 3. Red Team na escolha

Antes de fixar: "o que quebra essa stack em 12 meses? Que suposição, se falsa,
invalida a escolha?" Registre a resposta no ADR.

### 4. Gerar os 4 componentes

Ver `references/scaffold-*.md` para detalhes de cada componente:

- **Schema + migrations** → `references/scaffold-schema-migrations.md`
- **Matriz RBAC** → `references/scaffold-rbac.md`
- **Pagamento BR-first** → `references/scaffold-pagamento-br.md`
- **White-label** → `references/scaffold-white-label.md`

Gere de verdade — migration executável, matriz como tabela real do domínio,
webhook com idempotência implementada, tokens de tema parametrizados.

**Prometer "pronto pra escala" sem índice ou particionamento definido é proibido.**

### 5. Registrar ADR

Grave no `PROJECT_STATE.md` seção Decision Log, usando o template:

```
### ADR-00X: [decisão em 1 linha]
Data: [data]
Contexto: [as 3 respostas da Triagem]
Decisão: [stack ou padrão escolhido]
Alternativas descartadas: [as outras opções da tabela + por quê]
O que invalida essa escolha: [resposta do Red Team]
```

### 6. Devolver ao VibeDev

Sub-tarefa marcada como pronta para `/vd-check`. **Não avance de fase sozinho** —
isso é gate do VibeDev, não deste módulo.

### 7. Incrementar métrica (se aplicável)

Se a stack escolhida na Triagem for revertida depois, incremente
`stack_trocada` em `Métricas do framework` no `PROJECT_STATE.md`. Sinal de
que a Triagem está errando — se subir, revisar as 3 perguntas antes de
revisar o código.

---

## ⚠️ Quando NÃO usar este módulo

- Sub-tarefa pequena e claramente Tipo 2 (script isolado, automação pontual,
  ajuste que não toca fundação) → `/vd-build` direto, sem passar por aqui.
- Chamar o scaffold pra tarefa trivial é o mesmo erro que ele existe pra
  evitar do lado oposto: **fricção onde não precisa**.

---

## 🔗 Ver também

- `/vd-build` — comando-pai (este é sub-comando)
- `/vd-plan` — onde a sub-tarefa é aprovada antes de vir pra cá
- `/vd-devils-advocate` (v3.8) — gate disparado quando triagem discorda do PROJECT_STATE
- `/vd-check` — valida artefato gerado (inclui gate H2 de design tokens)
- `references/scaffold-stack-validada.md` — tabela de stacks com perfil Cynefin
- `references/scaffold-schema-migrations.md` — como gerar schema real
- `references/scaffold-rbac.md` — como gerar matriz RBAC
- `references/scaffold-pagamento-br.md` — como integrar PIX/MP/Asaas
- `references/scaffold-white-label.md` — como parametrizar tema
- `references/scaffold-legado-coexistencia.md` — caso especial WordPress/ERP

---

## 📚 Inspirações

- **Cynefin Framework** (Dave Snowden, 1999) — mapeamento de domínio de decisão
- **Dante Testa — Prompt Mestre WP v1.14.0** (jul/2026) — gerador de fundação
  com pagamento próprio e webhook idempotente
- **Rails Doctrine** (DHH, ~2011) — "convention over configuration" adaptado
  para triagem explícita
- **12 Factor App** (Adam Wiggins, 2011) — base para RBAC e config de pagamento
- **LGPD Art. 46** (Brasil, 2020) — segurança no tratamento de dados de pagamento
- **PCI-DSS 4.0** (2024) — base para webhook idempotente e segregação de dados sensíveis
