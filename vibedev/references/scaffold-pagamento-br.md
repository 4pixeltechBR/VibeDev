# Pagamento BR-First — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Como integrar pagamento **primário no Brasil** (PIX + Mercado Pago + Asaas)
> com Stripe como fallback. Sub-comando de `/vd-scaffold`. Webhook idempotente
> é **obrigatório** — cobrança duplicada em retry é bug de produção.

## 🇧🇷 Regra de seleção

| Sinal do usuário | Provider primário | Fallback |
|------------------|-------------------|----------|
| Cliente só no Brasil, menciona PIX | **PIX (via Asaas ou Mercado Pago)** | Stripe (cartão) |
| Cliente no Brasil + internacional | **Mercado Pago** (PIX + cartão + boleto) | Stripe (internacional) |
| Cliente só internacional, menciona Stripe | **Stripe** | Mercado Pago |
| Sem sinal claro | **Mercado Pago** (cobre 80% dos casos BR) | Stripe |

**Sempre** gerar suporte aos 3 providers BR mesmo se o primário for um só.
Razão: redundância operacional (provider fora do ar → outro assume).

## 🇺🇸 Selection rule

| User signal | Primary provider | Fallback |
|-------------|------------------|----------|
| Brazil only, mentions PIX | **PIX (via Asaas or Mercado Pago)** | Stripe (card) |
| Brazil + international | **Mercado Pago** (PIX + card + boleto) | Stripe (international) |
| International only, mentions Stripe | **Stripe** | Mercado Pago |
| No clear signal | **Mercado Pago** (covers 80% BR cases) | Stripe |

**Always** generate support for all 3 BR providers even if primary is only one.
Reason: operational redundancy (provider down → other takes over).

---

## 📐 Schema mínimo (BR-first)

```sql
-- Tabela de clientes no provider (não armazenar dados sensíveis de cartão!)
CREATE TABLE payment_customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    provider VARCHAR(50) NOT NULL,  -- 'mercadopago' | 'stripe' | 'asaas'
    provider_customer_id VARCHAR(255) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(tenant_id, user_id, provider)
);

-- Tabela de assinaturas
CREATE TABLE subscriptions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    provider VARCHAR(50) NOT NULL,
    provider_subscription_id VARCHAR(255) NOT NULL,
    plan_id VARCHAR(100) NOT NULL,
    status VARCHAR(50) NOT NULL,  -- 'active' | 'past_due' | 'canceled' | 'pending'
    current_period_start TIMESTAMPTZ NOT NULL,
    current_period_end TIMESTAMPTZ NOT NULL,
    cancel_at_period_end BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    deleted_at TIMESTAMPTZ
);

-- Tabela de eventos de webhook (idempotência!)
CREATE TABLE webhook_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    provider VARCHAR(50) NOT NULL,
    event_id VARCHAR(255) NOT NULL,  -- ID único do provider
    event_type VARCHAR(100) NOT NULL,
    payload JSONB NOT NULL,
    processed_at TIMESTAMPTZ,
    received_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(provider, event_id)  -- <<< IDEMPOTÊNCIA AQUI
);

-- Tabela de faturas
CREATE TABLE invoices (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL REFERENCES tenants(id) ON DELETE CASCADE,
    subscription_id UUID REFERENCES subscriptions(id) ON DELETE SET NULL,
    provider VARCHAR(50) NOT NULL,
    provider_invoice_id VARCHAR(255) NOT NULL,
    amount_cents BIGINT NOT NULL,
    currency CHAR(3) NOT NULL DEFAULT 'BRL',
    status VARCHAR(50) NOT NULL,  -- 'pending' | 'paid' | 'failed' | 'refunded'
    paid_at TIMESTAMPTZ,
    due_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

---

## 🔑 Webhook idempotente (obrigatório)

```python
# FastAPI exemplo
from fastapi import APIRouter, Request, HTTPException
from sqlalchemy import select
from app.models import WebhookEvent
from app.db import SessionDep

router = APIRouter()

@router.post("/webhooks/{provider}")
async def webhook(provider: str, request: Request, db: SessionDep):
    # 1. Ler payload cru (importante — assinatura depende do body original)
    raw_body = await request.body()
    payload = await request.json()

    # 2. Verificar assinatura do provider
    if not verify_signature(provider, raw_body, request.headers):
        raise HTTPException(401, "Invalid signature")

    # 3. Extrair event_id (cada provider tem o seu)
    event_id = extract_event_id(provider, payload)
    event_type = extract_event_type(provider, payload)

    # 4. IDEMPOTÊNCIA — checar se já processamos este event_id
    stmt = select(WebhookEvent).where(
        WebhookEvent.provider == provider,
        WebhookEvent.event_id == event_id
    )
    existing = (await db.execute(stmt)).scalar_one_or_none()

    if existing and existing.processed_at is not None:
        # Já processado — retorna 200 sem reprocessar
        return {"status": "already_processed"}

    # 5. Criar ou atualizar registro do evento
    if not existing:
        event = WebhookEvent(
            provider=provider,
            event_id=event_id,
            event_type=event_type,
            payload=payload,
            received_at=datetime.utcnow(),
        )
        db.add(event)
    else:
        existing.payload = payload
        existing.event_type = event_type

    # 6. Processar evento (atualizar subscription, invoice, etc)
    await process_webhook_event(provider, event_type, payload, db)

    # 7. Marcar como processado
    if existing:
        existing.processed_at = datetime.utcnow()
    else:
        event.processed_at = datetime.utcnow()

    await db.commit()

    return {"status": "processed"}
```

**Por que idempotência é obrigatória:**
- Provider reenvia webhook em caso de timeout (mesmo se processamos com sucesso)
- Retry mal configurado pode disparar 2x o mesmo evento
- Sem idempotência: 1 assinatura vira 2 cobranças, cliente cancela, reputação vai pro espaço

---

## 💰 PIX — particularidades BR

| Aspecto | Detalhe |
|---------|---------|
| Confirmação | PIX é instantâneo, mas webhook pode atrasar 5-30s |
| QR Code | Provider retorna `qr_code_base64` + `qr_code` (texto copia-e-cola) |
| Expiração | QR Code PIX expira em 30min a 24h (depende do provider) |
| Devolução | Estorno PIX é possível mas demora 1-7 dias úteis |
| Reconciliação | Job que compara `webhook_events` com `invoices` onde `status=pending` há mais de 1h |

```python
# Job de reconciliação (rodar a cada 1h via Celery/cron)
async def reconcile_pending_pix_invoices(db: SessionDep):
    """Para cada invoice PIX pendente há mais de 1h, consulta provider."""
    cutoff = datetime.utcnow() - timedelta(hours=1)
    stmt = select(Invoice).where(
        Invoice.status == "pending",
        Invoice.due_at < cutoff,
        Invoice.provider.in_(["asaas", "mercadopago"]),
    )
    pending = (await db.execute(stmt)).scalars().all()

    for invoice in pending:
        status = await check_provider_status(invoice.provider, invoice.provider_invoice_id)
        if status != invoice.status:
            invoice.status = status
            if status == "paid":
                invoice.paid_at = datetime.utcnow()
            await db.commit()
```

---

## 🏦 Mercado Pago — particularidades

| Aspecto | Detalhe |
|---------|---------|
| SDK | `mercadopago` (Python) ou `mercadopago-js` (frontend para Payment Brick) |
| Webhook URL | Configurar no painel MP, aponta para `/webhooks/mercadopago` |
| Assinatura | HMAC SHA256 do body com secret do MP |
| PIX | Suportado nativamente (modo `payment_method_id: "pix"`) |
| Cartão | Parcelamento em até 12x sem juros (config no painel MP) |
| Boleto | Suportado, compensação em 1-3 dias úteis |

---

## 🏦 Asaas — particularidades

| Aspecto | Detalhe |
|---------|---------|
| SDK | `asaas` (Python) ou REST direto |
| Foco | PIX + Boleto + Cartão, mais simples que MP |
| Assinatura | HMAC do header `asaas-access-token` (não do body) |
| Webhook | Configurar token no painel Asaas |
| Vantagem | API mais limpa, documentação melhor que MP pra PIX |

---

## 💳 Stripe (fallback internacional)

| Aspecto | Detalhe |
|---------|---------|
| SDK | `stripe` (Python) ou `@stripe/stripe-js` (frontend) |
| Webhook | Endpoint com assinatura via `Stripe-Signature` header |
| PIX no Stripe | Suportado desde 2024, mas cobertura BR ainda parcial |
| Cartão internacional | Forte, melhor que MP fora do BR |
| Use quando | Cliente tem CNPJ internacional, ou produto é global-first |

---

## 🔍 Checklist de validação (rodar antes de devolver ao `/vd-check`)

- [ ] Tabela `webhook_events` tem UNIQUE em `(provider, event_id)`
- [ ] Webhook handler checa `event_id` antes de processar (idempotência)
- [ ] Webhook handler verifica assinatura do provider
- [ ] Nenhum dado sensível de cartão é armazenado localmente (PCI-DSS)
- [ ] Tabela `invoices` registra `paid_at` quando status muda pra `paid`
- [ ] Job de reconciliação existe pra PIX pendente há mais de 1h
- [ ] Pelo menos 2 providers BR estão implementados (Mercado Pago + Asaas é mínimo)
- [ ] Fallback (Stripe) está configurado se houver sinal de cliente internacional
- [ ] Webhook endpoint tem rate limit (evita DDoS)
- [ ] Logs estruturados de webhook (provider, event_id, event_type, status) — LGPD-safe

---

## ⚠️ Anti-padrões (bloqueados pelo `/vd-check`)

- ❌ Webhook sem idempotência
- ❌ Webhook sem verificação de assinatura
- ❌ Armazenar número de cartão / CVV localmente (PCI-DSS viola)
- ❌ `amount` em `Float` (sempre `Integer` em centavos)
- ❌ Webhook sem rate limit
- ❌ Provider único sem fallback
- ❌ Reconciliação ausente pra PIX
- ❌ Logs de webhook com PII (email, nome) — LGPD Art. 46
- ❌ `currency` hardcoded (deve ser coluna)

---

## 🔒 LGPD e PCI-DSS

- **LGPD Art. 46** (Brasil): "medidas de segurança adequadas pra proteção de dados pessoais"
  - Logs de webhook: hash do email, não email puro
  - Auditoria: quem viu qual fatura? (campo `viewed_by` ou log de acesso)
- **PCI-DSS 4.0** (2024):
  - Nunca armazenar CVV (nem hash)
  - Armazenar últimos 4 dígitos só se necessário (recomenda-se token do provider)
  - TLS 1.2+ em toda comunicação
  - Logs de acesso a dados de cartão retidos por 1 ano mínimo

---

## 🔗 Ver também

- `scaffold-stack-validada.md` — qual stack
- `scaffold-schema-migrations.md` — schema base
- `scaffold-rbac.md` — RBAC pra separar quem vê faturas
- `references/lgpd-br.md` — compliance LGPD detalhado
- `/vd-check` — gate que valida este artefato
- **G1 do VibeShield** — "segredo exposto em código" (cobre API key do provider)

---

## 📚 Inspirações

- **PCI-DSS 4.0** (2024) — base de segurança
- **LGPD Art. 46** (Brasil, 2020) — base de privacidade
- **Mercado Pago DevSite** — docs oficiais
- **Asaas Docs** — docs oficiais
- **Stripe Webhook Best Practices** — base de idempotência
- **Dante Testa — Prompt Mestre WP v1.14.0** (jul/2026) — "pagamentos próprios sem WooCommerce"
