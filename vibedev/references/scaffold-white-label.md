# White-Label — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Como parametrizar tema, logo e domínio por tenant. Sub-comando de
> `/vd-scaffold`. **Proibido hardcode de cor ou marca em componente.**

## 🇧🇷 Princípio

White-label não é "tema padrão do framework". É um **conjunto de tokens**
que cada tenant sobrescreve. Toda referência visual no código aponta pra
`var(--token)`, nunca pra valor literal.

## 🇺🇸 Principle

White-label is not "framework default theme". It's a **set of tokens** that
each tenant overrides. Every visual reference in code points to `var(--token)`,
never to a literal value.

---

## 📐 Schema de tenant (extensão do `scaffold-schema-migrations.md`)

```sql
-- Tabela principal de tenants
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    slug VARCHAR(50) NOT NULL UNIQUE,  -- 'acme', 'beta-corp' (usado em subdomínio)
    name VARCHAR(255) NOT NULL,
    status VARCHAR(50) NOT NULL DEFAULT 'active',  -- 'active' | 'suspended' | 'canceled'
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    deleted_at TIMESTAMPTZ
);

-- Tokens de tema por tenant
CREATE TABLE tenant_themes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    token_name VARCHAR(100) NOT NULL,  -- 'color-primary', 'logo-url', etc
    token_value TEXT NOT NULL,
    token_type VARCHAR(50) NOT NULL DEFAULT 'string',  -- 'string' | 'color' | 'url' | 'font'
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(tenant_id, token_name)
);

-- Domínios customizados (white-label completo)
CREATE TABLE tenant_domains (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    domain VARCHAR(255) NOT NULL UNIQUE,  -- 'app.acme.com.br'
    is_primary BOOLEAN NOT NULL DEFAULT false,
    verified_at TIMESTAMPTZ,
    ssl_status VARCHAR(50) NOT NULL DEFAULT 'pending',  -- 'pending' | 'active' | 'failed'
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

---

## 🎨 Tokens obrigatórios

| Token | Tipo | Default | Onde é usado |
|-------|------|---------|--------------|
| `color-primary` | color | `#3B82F6` (azul VibeDev) | Botões principais, links |
| `color-primary-hover` | color | `#2563EB` | Hover de botões |
| `color-secondary` | color | `#10B981` (verde) | Badges de sucesso, CTAs secundários |
| `color-danger` | color | `#EF4444` | Erros, botões destrutivos |
| `color-warning` | color | `#F59E0B` | Avisos |
| `color-background` | color | `#FFFFFF` | Fundo da página |
| `color-surface` | color | `#F9FAFB` | Cards, modais |
| `color-text-primary` | color | `#111827` | Texto principal |
| `color-text-secondary` | color | `#6B7280` | Texto secundário |
| `color-border` | color | `#E5E7EB` | Bordas |
| `font-family-base` | font | `Inter, system-ui, sans-serif` | Fonte padrão |
| `font-family-heading` | font | `Inter, system-ui, sans-serif` | Títulos |
| `logo-url` | url | `/assets/logo-default.svg` | Logo no header |
| `logo-dark-url` | url | `/assets/logo-default-dark.svg` | Logo em dark mode |
| `favicon-url` | url | `/favicon.ico` | Favicon |
| `domain` | url | tenant.slug.domain | Domínio customizado |
| `support-email` | string | `support@vibedev.local` | Email de suporte |
| `company-name` | string | `VibeDev` | Nome da empresa exibido |

---

## 🔧 Implementação agnóstica

### Opção A: CSS Variables (frontend puro, sem framework)

```css
/* styles/tokens.css */
:root {
  --color-primary: #3B82F6;
  --color-primary-hover: #2563EB;
  /* ... outros tokens ... */
}

/* Quando tenant carrega, sobrescreve via JS */
[data-tenant="acme"] {
  --color-primary: #FF6B35;  /* laranja da Acme */
  --logo-url: url('/tenants/acme/logo.svg');
}

/* Componentes NUNCA usam cor literal */
.button-primary {
  background: var(--color-primary);  /* ✅ */
  /* background: #3B82F6; */        /* ❌ bloqueado pelo /vd-check */
}
```

```typescript
// lib/tenant.ts
export async function loadTenantTheme(tenantId: string) {
  const tokens = await api.get(`/tenants/${tenantId}/theme`);
  const root = document.documentElement;
  for (const [key, value] of Object.entries(tokens)) {
    root.style.setProperty(`--${key}`, value);
  }
}
```

### Opção B: Tailwind + plugin de tema dinâmico (frontend com Tailwind)

```typescript
// tailwind.config.ts
import type { Config } from "tailwindcss";

export default {
  theme: {
    extend: {
      colors: {
        primary: "var(--color-primary)",
        "primary-hover": "var(--color-primary-hover)",
        secondary: "var(--color-secondary)",
        // ...
      },
    },
  },
} satisfies Config;
```

```tsx
// Componente
<button className="bg-primary hover:bg-primary-hover text-white">
  Clique aqui
</button>
// ✅ Tailwind usa a var CSS, que é sobrescrita pelo tenant
```

### Opção C: Theme UI / styled-components (projetos com Emotion)

```typescript
// theme.ts
export const lightTheme = {
  colors: {
    primary: "var(--color-primary)",
    primaryHover: "var(--color-primary-hover)",
  },
};

// App.tsx
<ThemeProvider theme={lightTheme}>
  <Button color="primary">Clique aqui</Button>
</ThemeProvider>
```

---

## 🖼️ Logo e favicon (parametrizados)

```typescript
// components/Logo.tsx
interface LogoProps {
  className?: string;
}

export function Logo({ className }: LogoProps) {
  // ❌ ERRADO: <img src="/assets/logo-default.svg" />
  // ✅ CORRETO: usa token CSS
  return (
    <img
      src={getToken("logo-url", "/assets/logo-default.svg")}
      alt={getToken("company-name", "VibeDev")}
      className={className}
    />
  );
}
```

```html
<!-- index.html (dinâmico) -->
<script>
  // Carregado pelo SSR ou client-side antes do paint
  const tokens = window.__TENANT_TOKENS__;
  document.documentElement.dataset.tenant = tokens.slug;
  // Favicon dinâmico
  const favicon = document.querySelector('link[rel="icon"]') ||
                  document.createElement('link');
  favicon.rel = "icon";
  favicon.href = tokens.faviconUrl;
  document.head.appendChild(favicon);
</script>
```

---

## 🌐 Domínio customizado

### Fluxo de provisionamento

```python
# 1. Tenant registra domínio
async def add_custom_domain(tenant_id: UUID, domain: str, db: SessionDep):
    # Validar formato
    if not is_valid_domain(domain):
        raise HTTPException(400, "Invalid domain format")

    # Verificar se domínio já existe
    existing = await db.execute(
        select(TenantDomain).where(TenantDomain.domain == domain)
    )
    if existing.scalar_one_or_none():
        raise HTTPException(409, "Domain already in use")

    # Criar registro (pending verification)
    tenant_domain = TenantDomain(
        tenant_id=tenant_id,
        domain=domain,
        is_primary=False,
        ssl_status="pending",
    )
    db.add(tenant_domain)
    await db.commit()

    # Retornar instruções de DNS
    return {
        "domain": domain,
        "dns_records": [
            {"type": "CNAME", "name": domain, "value": "app.vibedev.com.br"},
            {"type": "TXT", "name": f"_vibedev-verify.{domain}", "value": f"vibedev-verify={generate_token()}"},
        ],
    }

# 2. Job verifica DNS periodicamente
async def verify_custom_domain(tenant_domain_id: UUID, db: SessionDep):
    td = await db.get(TenantDomain, tenant_domain_id)

    # Verificar TXT record de validação
    if check_txt_record(f"_vibedev-verify.{td.domain}", expected_token):
        td.verified_at = datetime.utcnow()

    # Provisionar SSL (via Caddy, Let's Encrypt, ou Cloudflare)
    if td.verified_at and td.ssl_status == "pending":
        success = provision_ssl(td.domain)
        td.ssl_status = "active" if success else "failed"

    await db.commit()
```

---

## 🔍 Checklist de validação (rodar antes de devolver ao `/vd-check`)

- [ ] Tabela `tenant_themes` tem índice UNIQUE em `(tenant_id, token_name)`
- [ ] Todo componente frontend usa `var(--token)` ou `getToken()`, **nunca** valor literal
- [ ] Tokens de cor, fonte, logo, favicon e domínio estão parametrizados
- [ ] Domínio customizado tem fluxo de verificação DNS + provisionamento SSL
- [ ] Pelo menos 1 tema de exemplo está seed no banco (default VibeDev)
- [ ] Modo dark e modo light ambos parametrizaveis (se o app suportar)
- [ ] Email de suporte, nome de empresa, etc são tokens (não hardcoded)
- [ ] Logs de erro não vazam tokens (cuidado com `console.log(theme)`)

---

## ⚠️ Anti-padrões (bloqueados pelo `/vd-check` — gate H2)

- ❌ `background: #3B82F6` em qualquer arquivo CSS/SCSS/Styled
- ❌ `color: 'blue'` hardcoded em componente React/Vue/Svelte
- ❌ `<img src="/assets/logo-default.svg" />` em vez de token
- ❌ `font-family: 'Inter'` literal em CSS (deveria ser `var(--font-family-base)`)
- ❌ Domínio `app.empresa.com` hardcoded em variável de ambiente (deveria ser `tenant.domain`)
- ❌ `support@vibedev.com` literal em email (deveria ser token)
- ❌ Componente que renderiza nome da empresa hardcoded

---

## 🔒 LGPD e privacidade

- **Logos e favicons** podem conter imagem de pessoa (foto do fundador)?
  - Se sim, expor isso na Política de Privacidade do tenant
  - Tenant é responsável pelo asset que sobe
- **Nome de empresa** é dado pessoal só se for nome de pessoa física (MEI)
  - Em caso de MEI, exibir com cuidado (consentimento)

---

## 🔗 Ver também

- `scaffold-stack-validada.md` — qual stack
- `scaffold-schema-migrations.md` — schema base
- `scaffold-rbac.md` — quem pode editar tema do tenant
- `scaffold-pagamento-br.md` — white-label vs pagamento (são ortogonais)
- `/vd-check` — gate que valida este artefato (inclui H2 de design tokens)
- **H2 do VibeShield** (proposta) — heurística de design tokens

---

## 📚 Inspirações

- **CSS Custom Properties (CSS Variables)** — base da Opção A
- **Tailwind CSS Dynamic Theming** — base da Opção B
- **Stripe Brand Guidelines** — exemplo de white-label bem feito
- **WordPress Multisite** — base de multi-tenant + white-label
- **Shopify Themes** — tokens como contrato entre plataforma e tema
- **Dante Testa — Prompt Mestre WP v1.14.0** (jul/2026) — "identidade visual white-label"
