# Schema + Migrations — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Como gerar schema real e migrations versionadas, agnóstico de stack
> (FastAPI/SQLAlchemy, Next.js/Drizzle, ou outras). Sub-comando de `/vd-scaffold`.

## 🇧🇷 Regras obrigatórias

1. **Modelo de dados com os relacionamentos reais do domínio descrito**, nunca genérico.
2. **Migration versionada e reversível (up/down)** — nunca `CREATE TABLE` solto
   sem controle de versão.
3. **Índice em toda coluna usada em filtro/JOIN de consulta frequente.** Se o
   perfil da Triagem foi "escala incerta pra cima", já modele particionamento
   por data ou por `tenant_id` desde a primeira tabela grande — trocar depois
   é Tipo 1 caro.
4. **Multi-tenancy:** se a stack escolhida for SaaS multi-cliente, toda tabela
   de domínio (exceto `tenants`, `users`, `subscriptions`) tem `tenant_id` como
   primeira coluna após o `id`, e índice composto `(tenant_id, ...)`.
5. **Soft delete:** adicionar `deleted_at TIMESTAMP NULL` em toda tabela de
   domínio, com índice parcial `WHERE deleted_at IS NULL` para queries normais.
6. **Auditoria:** colunas `created_at`, `updated_at`, `created_by`, `updated_by`
   em toda tabela de domínio. Se LGPD for restrição, considerar `data_lgpd_*` para
   campos sensíveis.

## 🇺🇸 Mandatory rules

1. **Data model with the actual domain relationships described**, never generic.
2. **Versioned and reversible migration (up/down)** — never loose `CREATE TABLE`
   without version control.
3. **Index on every column used in frequent filter/JOIN.** If Triage profile was
   "scale uncertain upwards", model partitioning by date or by `tenant_id` from
   the first big table — swapping later is expensive Type 1.
4. **Multi-tenancy:** if chosen stack is multi-tenant SaaS, every domain table
   (except `tenants`, `users`, `subscriptions`) has `tenant_id` as first column
   after `id`, with composite index `(tenant_id, ...)`.
5. **Soft delete:** add `deleted_at TIMESTAMP NULL` to every domain table, with
   partial index `WHERE deleted_at IS NULL` for normal queries.
6. **Audit:** `created_at`, `updated_at`, `created_by`, `updated_by` columns on
   every domain table. If LGPD is a constraint, consider `data_lgpd_*` for
   sensitive fields.

---

## 📐 Template de migration (agnóstico)

### Alembic (FastAPI + SQLAlchemy)

```python
# alembic/versions/001_create_users.py
"""create users table

Revision ID: 001
Revises:
Create Date: 2026-07-26 02:30:00
"""
from alembic import op
import sqlalchemy as sa
import sqlalchemy.dialects.postgresql as pg

revision = "001"
down_revision = None
branch_labels = None
depends_on = None


def upgrade() -> None:
    op.create_table(
        "users",
        sa.Column("id", pg.UUID(as_uuid=True), primary_key=True, server_default=sa.text("gen_random_uuid()")),
        sa.Column("tenant_id", pg.UUID(as_uuid=True), sa.ForeignKey("tenants.id", ondelete="CASCADE"), nullable=False),
        sa.Column("email", sa.String(255), nullable=False),
        sa.Column("password_hash", sa.String(255), nullable=False),
        sa.Column("name", sa.String(255), nullable=False),
        sa.Column("role", sa.String(50), nullable=False, server_default="viewer"),
        sa.Column("is_active", sa.Boolean, nullable=False, server_default=sa.true()),
        sa.Column("created_at", sa.TIMESTAMP(timezone=True), nullable=False, server_default=sa.text("now()")),
        sa.Column("updated_at", sa.TIMESTAMP(timezone=True), nullable=False, server_default=sa.text("now()")),
        sa.Column("created_by", pg.UUID(as_uuid=True), sa.ForeignKey("users.id"), nullable=True),
        sa.Column("updated_by", pg.UUID(as_uuid=True), sa.ForeignKey("users.id"), nullable=True),
        sa.Column("deleted_at", sa.TIMESTAMP(timezone=True), nullable=True),
    )
    # Indexes
    op.create_index("ix_users_tenant_id", "users", ["tenant_id"])
    op.create_index("ix_users_email", "users", ["email"], unique=True)
    op.create_index("ix_users_active", "users", ["tenant_id"], postgresql_where=sa.text("deleted_at IS NULL"))


def downgrade() -> None:
    op.drop_index("ix_users_active", table_name="users")
    op.drop_index("ix_users_email", table_name="users")
    op.drop_index("ix_users_tenant_id", table_name="users")
    op.drop_table("users")
```

### Drizzle (Next.js + SQLite/Postgres)

```typescript
// drizzle/0001_create_users.ts
import { pgTable, uuid, varchar, boolean, timestamp, index, uniqueIndex } from "drizzle-orm/pg-core";
import { sql } from "drizzle-orm";
import { tenants } from "./0000_create_tenants";

export const users = pgTable("users", {
  id: uuid("id").primaryKey().defaultRandom(),
  tenantId: uuid("tenant_id").notNull().references(() => tenants.id, { onDelete: "cascade" }),
  email: varchar("email", { length: 255 }).notNull(),
  passwordHash: varchar("password_hash", { length: 255 }).notNull(),
  name: varchar("name", { length: 255 }).notNull(),
  role: varchar("role", { length: 50 }).notNull().default("viewer"),
  isActive: boolean("is_active").notNull().default(true),
  createdAt: timestamp("created_at", { withTimezone: true }).notNull().defaultNow(),
  updatedAt: timestamp("updated_at", { withTimezone: true }).notNull().defaultNow(),
  createdBy: uuid("created_by").references(() => users.id),
  updatedBy: uuid("updated_by").references(() => users.id),
  deletedAt: timestamp("deleted_at", { withTimezone: true }),
}, (table) => ({
  tenantIdx: index("ix_users_tenant_id").on(table.tenantId),
  emailUq: uniqueIndex("ix_users_email").on(table.email),
  activeIdx: index("ix_users_active").on(table.tenantId).where(sql`deleted_at IS NULL`),
}));
```

---

## 🔍 Checklist de validação (rodar antes de devolver ao `/vd-check`)

- [ ] Migration tem `upgrade()` E `downgrade()` (reversível)
- [ ] Toda tabela de domínio tem `id`, `created_at`, `updated_at`, `deleted_at`
- [ ] Toda FK tem `ondelete` definido (`CASCADE` ou `RESTRICT`, nunca omitido)
- [ ] Toda coluna usada em WHERE/JOIN frequente tem índice
- [ ] Se multi-tenant: toda tabela de domínio tem `tenant_id` com índice composto
- [ ] Email é UNIQUE (não só indexado)
- [ ] Senha é `password_hash` (nunca `password` em texto puro)
- [ ] Soft delete tem índice parcial `WHERE deleted_at IS NULL`
- [ ] Migration roda limpa em DB vazio (`alembic upgrade head` ou `drizzle-kit push`)
- [ ] Migration reverte limpa (`alembic downgrade -1` ou `drizzle-kit drop`)

---

## ⚠️ Anti-padrões (bloqueados pelo `/vd-check`)

- ❌ `id` como `Integer` autoincrement (use UUID)
- ❌ `created_at` sem timezone
- ❌ FK sem `ondelete`
- ❌ Tabela sem `tenant_id` em contexto multi-tenant
- ❌ Migration sem `downgrade()`
- ❌ Coluna JSON sem schema definido (Pydantic/Zod)
- ❌ Senha em texto puro
- ❌ Email duplicável

---

## 🔗 Ver também

- `scaffold-stack-validada.md` — qual stack escolher
- `scaffold-rbac.md` — modelo de permissões (depende deste schema)
- `references/decisao-stack.md` — registro no Decision Log
- `/vd-check` — gate que valida este artefato
