# Stack Validada — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Tabela de stacks pré-validadas para o motor `/vd-scaffold`, baseadas em
> Cynefin-lite. **Não é whitelist** — caso de borda vai pra Tipo 1 de alta
> incerteza, com 3 opções apresentadas ao usuário.

## 🇧🇷 As 3 stacks (padrão)

| Perfil Cynefin | Stack | Quando faz sentido | Tradeoff principal |
|----------------|-------|-------------------|-------------------|
| **Complicado** (SaaS real / multi-cliente, escala incerta pra cima) | FastAPI + React + TS + Postgres (+ Redis se fila/cache justificar) | Padrão consistente com projetos local-first em uso — contratos Pydantic, sem vendor lock-in | Custo inicial maior; precisa de DevOps desde o dia 1 |
| **Simples** (MVP de validação rápida) | Next.js fullstack + SQLite (migra pra Postgres se validar) | Time-to-mercado importa mais que escala ainda não comprovada | SQLite em prod = armadilha; migração tem que estar no roadmap desde o commit 1 |
| **Caótico/Complicado** (legado coexistindo) | **Tratar como Tipo 1 de alta incerteza** — apresentar 3 opções, perguntar | Migrar sairia mais caro que integrar; o legado é restrição externa | Acopla a dois sistemas; dívida técnica cresce |

## 🇺🇸 The 3 stacks (default)

| Cynefin profile | Stack | When it makes sense | Main tradeoff |
|-----------------|-------|---------------------|---------------|
| **Complicated** (real SaaS / multi-client, scale uncertain upwards) | FastAPI + React + TS + Postgres (+ Redis if queue/cache justifies) | Default consistent with local-first projects in use — Pydantic contracts, no vendor lock-in | Higher initial cost; needs DevOps from day 1 |
| **Simple** (quick validation MVP) | Next.js fullstack + SQLite (migrate to Postgres if validated) | Time-to-market matters more than unproven scale | SQLite in prod = trap; migration must be in roadmap from commit 1 |
| **Chaotic/Complicated** (legacy coexisting) | **Treat as Type 1 high uncertainty** — present 3 options, ask | Migrating out would be more expensive than integrating; legacy is external constraint | Couples two systems; technical debt grows |

---

## 📐 Detalhamento da stack padrão SaaS (FastAPI + React + TS + Postgres)

### Backend
- **Linguagem:** Python 3.12+ (type hints estritos)
- **Framework:** FastAPI 0.115+ (async, OpenAPI nativo)
- **ORM:** SQLAlchemy 2.0+ (com `Mapped` types) ou SQLModel
- **Migrations:** Alembic (gerado pelo SQLAlchemy)
- **Validação:** Pydantic v2 (contratos tipados em entrada/saída)
- **Auth:** JWT + refresh token, com rotação e revogação
- **Testes:** pytest + httpx (async test client)
- **Estrutura de pastas:**
  ```
  backend/
  ├── app/
  │   ├── main.py
  │   ├── core/        (config, security, dependencies)
  │   ├── models/      (SQLAlchemy)
  │   ├── schemas/     (Pydantic)
  │   ├── api/         (rotas, agrupadas por recurso)
  │   ├── services/    (lógica de domínio)
  │   └── db/          (sessão, base, migrations)
  ├── tests/
  ├── alembic/
  ├── pyproject.toml
  └── .env.example
  ```

### Frontend
- **Linguagem:** TypeScript 5.5+ (strict mode)
- **Framework:** React 18+ ou 19+
- **Build:** Vite 5+
- **Styling:** Tailwind 3+ (com design tokens)
- **Roteamento:** React Router 6+ ou TanStack Router
- **State:** TanStack Query (server state) + Zustand (client state)
- **Forms:** React Hook Form + Zod (validação)
- **Testes:** Vitest + Testing Library
- **Estrutura de pastas:**
  ```
  frontend/
  ├── src/
  │   ├── main.tsx
  │   ├── App.tsx
  │   ├── routes/      (páginas, agrupadas por recurso)
  │   ├── components/  (componentes reutilizáveis)
  │   ├── hooks/       (hooks customizados)
  │   ├── lib/         (utilitários, API client)
  │   ├── styles/      (tokens de tema, Tailwind config)
  │   └── types/       (tipos compartilhados)
  ├── tests/
  ├── package.json
  └── .env.example
  ```

### Banco
- **Postgres 16+** com extensões úteis:
  - `pgcrypto` (UUIDs)
  - `pg_trgm` (busca textual)
  - `citext` (case-insensitive text)
  - `pg_stat_statements` (observabilidade)
- **Particionamento:** por `tenant_id` ou por `created_at` (data) desde a primeira tabela grande, se perfil for "escala incerta pra cima"

### Cache/Fila (opt-in)
- **Redis 7+** se a aplicação tiver:
  - Fila assíncrona (Celery, RQ, ou Dramatiq)
  - Cache de sessão
  - Rate limiting distribuído
- **Sem Redis se:** API é 100% síncrona, sem fila, sem rate limit distribuído

---

## 📐 Detalhamento da stack MVP (Next.js fullstack + SQLite)

### Stack
- **Next.js 15+** com App Router
- **TypeScript 5.5+** strict
- **Drizzle ORM** (TypeScript-first, leve, suporta SQLite nativo)
- **Auth:** NextAuth.js / Auth.js (com provider de email + magic link)
- **DB:** SQLite via `better-sqlite3` (síncrono, simples, rápido)
- **Deploy:** Vercel (plano hobby gratuito no início)
- **Estrutura:** `app/` (rotas) + `lib/db/` (schema) + `drizzle/` (migrations)

### Plano de migração
- **Dia 1:** schema em Drizzle, agnóstico de DB
- **Dia X (validação OK):** trocar driver SQLite por Postgres
- **Sem retrabalho:** SQL Drizzle é portável, só muda conexão

---

## ⚠️ WordPress / ERP / sistemas legados (caso de borda)

**NÃO é uma das 3 stacks pré-validadas.** É um **caso de borda Cynefin** que
sempre dispara Tipo 1 de alta incerteza. Ver `scaffold-legado-coexistencia.md`.

Por quê não está na tabela:
- VibeDev é framework horizontal, agnóstico de stack
- Cada projeto com legado é único (qual ERP, qual versão, qual integração)
- Deixar na tabela pré-validada faria parecer que o framework "recomenda WordPress"
  — não recomenda

---

## 🔗 Ver também

- `scaffold-legado-coexistencia.md` — caso de borda
- `scaffold-schema-migrations.md` — como gerar schema em qualquer das 3 stacks
- `scaffold-rbac.md` — RBAC agnóstico
- `scaffold-pagamento-br.md` — pagamento agnóstico
- `scaffold-white-label.md` — white-label agnóstico
- `references/decisao-stack.md` — como registrar decisão no Decision Log

---

## 📚 Inspirações

- **Cynefin Framework** (Dave Snowden, 1999) — base dos perfis
- **Vercel Stack** (Next.js, 2024) — referência pra MVP
- **T3 Stack** (Theo Browne, 2023) — base da stack SaaS
- **12 Factor App** (Adam Wiggins, 2011) — config, dependências, processos
