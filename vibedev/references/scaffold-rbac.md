# Matriz RBAC — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Como gerar matriz RBAC real, não declaração solta. Sub-comando de
> `/vd-scaffold`. Tabela é do **domínio descrito**, não template genérico.

## 🇧🇷 Princípio

RBAC não é "lista de roles hardcoded". É uma **matriz 4-dimensional**:

| Role | Recurso | Ação | Escopo |

Gerada a partir do **domínio descrito** na sub-tarefa do `/vd-plan`. O
template abaixo é **exemplo**, não resposta final. Você sempre gera a versão
específica do domínio.

## 🇺🇸 Principle

RBAC is not "hardcoded role list". It's a **4-dimensional matrix**:

| Role | Resource | Action | Scope |

Generated from the **domain described** in the `/vd-plan` sub-task. The
template below is an **example**, not final answer. You always generate the
specific version of the domain.

---

## 📐 Estrutura da matriz

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| Role | string | Quem exerce (admin, gestor, operador, viewer, custom) |
| Recurso | string | Sobre o que (clientes, relatórios, faturas, projetos, etc) |
| Ação | enum | `criar` · `ler` · `editar` · `deletar` · `aprovar` · `exportar` · `*` |
| Escopo | string | `próprio` · `time` · `organização` · `tenant` · `*` |

---

## 🧪 Exemplo de matriz (template — gerar a SUA)

> **⚠️ Isto é template. O agente SEMPRE gera a versão específica do domínio
> descrito. Nunca entregar este como resposta final.**

| Role | Recurso | Ação | Escopo |
|------|---------|------|--------|
| admin | * | * | organização |
| gestor | clientes | criar, ler, editar | próprio time |
| gestor | relatórios | ler, exportar | próprio time |
| operador | clientes | ler, editar | registros atribuídos a ele |
| operador | tarefas | criar, ler, editar, completar | registros atribuídos a ele |
| viewer | * | ler | próprio time |
| cliente | próprios dados | ler, editar | próprio registro |
| cliente | faturas | ler, pagar | próprio registro |

---

## 📋 Regras de geração

1. **Sempre incluir `admin` com `*` no escopo `organização`** — a menos que o
   domínio proíba explicitamente (ex: app sem admin é estranho)
2. **Roles de domínio são específicos do projeto** — não inventar 8 roles se o
   domínio tem 3 personas
3. **Escopo `próprio` requer coluna `owner_id` ou `user_id` na tabela** —
   senão é impossível aplicar
4. **Escopo `time` requer tabela `teams` + `team_members`** — modelar antes da RBAC
5. **Escopo `organização` ou `tenant` requer multi-tenancy** — modelar antes
6. **Wildcard `*` é exclusivo do `admin`** — não dar `*` pra outros roles
7. **Ação `deletar` em produção real é raramente boa** — preferir `soft delete`
   (escopo da action vira `marcar_deletado`)

---

## 🔧 Implementação agnóstica

### Opção A: Role como coluna simples (projetos pequenos, MVP)

```sql
-- Schema (já em scaffold-schema-migrations.md)
ALTER TABLE users ADD COLUMN role VARCHAR(50) NOT NULL DEFAULT 'viewer';
```

```python
# Python (FastAPI)
from enum import Enum

class Role(str, Enum):
    ADMIN = "admin"
    GESTOR = "gestor"
    OPERADOR = "operador"
    VIEWER = "viewer"

def check_permission(user: User, resource: str, action: str) -> bool:
    matrix = {
        Role.ADMIN: {"*": "*"},
        Role.GESTOR: {
            "clientes": ["criar", "ler", "editar"],
            "relatorios": ["ler", "exportar"],
        },
        Role.OPERADOR: {
            "clientes": ["ler", "editar"],
            "tarefas": ["criar", "ler", "editar", "completar"],
        },
        Role.VIEWER: {"*": ["ler"]},
    }
    user_perms = matrix.get(user.role, {})
    if "*" in user_perms and user_perms["*"] == "*":
        return True
    resource_perms = user_perms.get(resource, user_perms.get("*", []))
    return action in resource_perms
```

**Limitação:** scope (`próprio`, `time`) tem que ser checado em código de
serviço, não aqui. Pra projetos sérios, usar Opção B.

### Opção B: Tabela de permissões (projetos médios/grandes)

```sql
CREATE TABLE roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    name VARCHAR(50) NOT NULL,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(tenant_id, name)
);

CREATE TABLE permissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    role_id UUID NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    resource VARCHAR(100) NOT NULL,
    action VARCHAR(50) NOT NULL,
    scope VARCHAR(50) NOT NULL DEFAULT 'tenant',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(role_id, resource, action, scope)
);

CREATE TABLE user_roles (
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role_id UUID NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    granted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    granted_by UUID REFERENCES users(id),
    PRIMARY KEY (user_id, role_id)
);
```

**Vantagem:** admin pode criar/editar roles sem deploy.

### Opção C: Casbin / Oso (projetos complexos, multi-policy)

```python
# Casbin
import casbin

e = casbin.Enforcer("model.conf", "policy.csv")

def check_permission(user_id: str, resource: str, action: str) -> bool:
    return e.enforce(user_id, resource, action)
```

**Vantagem:** policy externalizável, auditável, hot-reload. Bom pra compliance
(LGPD, SOX, HIPAA).

---

## 🔍 Checklist de validação (rodar antes de devolver ao `/vd-check`)

- [ ] Matriz tem pelo menos 1 role `admin` com `*` em escopo `organização`
- [ ] Cada role tem pelo menos 1 (recurso, ação) definido
- [ ] Escopo `próprio` tem coluna `owner_id` ou `user_id` na tabela
- [ ] Escopo `time` tem tabela `teams` modelada
- [ ] Escopo `organização` ou `tenant` tem `tenant_id` em toda tabela de domínio
- [ ] Action `deletar` foi reconsiderada (preferir `marcar_deletado` + soft delete)
- [ ] Wildcard `*` é exclusivo do `admin`
- [ ] Implementação escolhida (A, B ou C) é apropriada ao tamanho do projeto
- [ ] Testes de permissão cobrem pelo menos: admin full, viewer read-only, scope próprio, scope time

---

## ⚠️ Anti-padrões (bloqueados pelo `/vd-check`)

- ❌ Matriz sem `admin` com `*`
- ❌ Role com `*` em escopo que não seja `organização`
- ❌ Wildcard `*` dado a role não-admin
- ❌ Escopo `próprio` sem coluna de ownership
- ❌ Escopo `time` sem modelagem de `teams`
- ❌ Action `deletar` sem soft delete
- ❌ Roles hardcoded em código sem tabela de permissões (em projetos >MVP)

---

## 🔗 Ver também

- `scaffold-stack-validada.md` — qual stack
- `scaffold-schema-migrations.md` — schema das tabelas envolvidas
- `/vd-arch-review-meso` — audita se RBAC está bem separado por camada
- `/vd-check` — gate que valida este artefato
