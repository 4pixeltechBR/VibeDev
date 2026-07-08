# Estimativa de Custo Mensal — Referência VibeDev

> Tabela de referência pra calcular custo recorrente ANTES de escolher stack.
> Carregada automaticamente na Fase 3 (Arquitetura), antes do gate pra Fase 4.
> Modo leigo: traduz valores em reais, sem jargão de pricing tier.

---

## Como usar

1. Identifique a **categoria** do projeto (linha 1 da tabela).
2. Identifique o **porte** esperado (coluna: 1-10 / 10-100 / 100-1000 usuários).
3. Some os custos das camadas (hospedagem, banco, auth, e-mail, pagamentos,
   observabilidade, domínio, etc.) conforme o que o produto precisa.
4. Registre no `PROJECT_STATE.md` no campo `custo_mensal_estimado`.
5. O gate Fase 3 → 4 **não passa** sem esse campo preenchido.

**Importante:** esses valores são ordem de grandeza, sem compromisso firme.
Para cotar, entre no site de cada serviço. Nada aqui substitui orçamento real.

---

## Tabela por categoria (BRL, estimado)

### Landing page simples (1 página, formulário de contato)

| Camada | Serviço comum | 1-100 visitas/mês | 100-5.000/mês | 5.000-50.000/mês |
|---|---|---|---|---|
| Hospedagem | Vercel / Netlify free tier | R$ 0 | R$ 0 | R$ 50-200 |
| Domínio | Registro.br | ~R$ 40/ano | ~R$ 40/ano | ~R$ 40/ano |
| E-mail transacional | Resend / SendGrid free | R$ 0 | R$ 0-50 | R$ 50-200 |
| **TOTAL estimado** | | **R$ 0-5/mês** | **R$ 0-50/mês** | **R$ 100-450/mês** |

### Site institucional (3-10 páginas, blog opcional)

| Camada | Serviço comum | 1-100 visitas/mês | 100-5.000/mês |
|---|---|---|---|
| Hospedagem | Vercel / Netlify | R$ 0-50 | R$ 50-200 |
| CMS (se necessário) | Sanity / Strapi / markdown | R$ 0 | R$ 0-100 |
| Domínio + SSL | Registro.br + Cloudflare | ~R$ 60/ano | ~R$ 60/ano |
| **TOTAL estimado** | | **R$ 0-50/mês** | **R$ 50-300/mês** |

### App interno / dashboard privado (login + CRUD, sem público)

| Camada | Serviço comum | 1-10 usuários | 10-100 | 100-1000 |
|---|---|---|---|---|
| Hospedagem | Railway / Render / Fly.io | R$ 25-100 | R$ 100-500 | R$ 500-3000 |
| Banco | Postgres gerenciado (Neon/Supabase) | R$ 0-50 | R$ 50-300 | R$ 300-2000 |
| Auth | Clerk / Supabase Auth | R$ 0 | R$ 100-500 | R$ 500-3000 |
| Observabilidade | Sentry + Logtail free tiers | R$ 0 | R$ 0-200 | R$ 200-1000 |
| **TOTAL estimado** | | **R$ 25-200/mês** | **R$ 250-1500/mês** | **R$ 1500-9000/mês** |

### SaaS B2C público (freemium, login, pagamento)

| Camada | 1-100 usuários | 100-1.000 | 1.000-10.000 |
|---|---|---|---|
| Hospedagem | R$ 100-500 | R$ 500-3000 | R$ 3000-15000 |
| Banco | R$ 0-100 | R$ 100-800 | R$ 800-5000 |
| Auth | R$ 0-100 | R$ 100-1000 | R$ 1000-5000 |
| E-mail transacional | R$ 0-50 | R$ 50-300 | R$ 300-1500 |
| Pagamento (Stripe) | % por transação | 2,9% + R$ 0,60 por Pix/cartão |
| Observabilidade | R$ 0-100 | R$ 100-500 | R$ 500-3000 |
| Suporte / atendimento | (tempo humano) | (tempo humano) | (tempo humano ou ferramenta) |
| **TOTAL estimado (sem %)** | **R$ 100-900/mês** | **R$ 850-5900/mês** | **R$ 5600-29500/mês** |

### Bot / agente em mensageria (Telegram, WhatsApp)

| Camada | 1-100 conversas/mês | 100-1000 | 1000-10000 |
|---|---|---|---|
| Hospedagem | R$ 25-100 | R$ 100-400 | R$ 400-2000 |
| Banco | R$ 0-50 | R$ 50-200 | R$ 200-1000 |
| Custo de API de IA | variável por uso | R$ 50-500 | R$ 500-8000 |
| Canal (WhatsApp Business API) | R$ 0-150 | R$ 150-1000 | R$ 1000-8000 |
| **TOTAL estimado** | **R$ 25-300/mês** | **R$ 350-2100/mês** | **R$ 2100-19000/mês** |

### App mobile (iOS + Android, loja)

| Custo | Observação |
|---|---|
| Apple Developer | ~R$ 320/ano (~US$ 99/ano) |
| Google Play Developer | ~R$ 80 única vez (~US$ 25) |
| Backend (mesmo que SaaS) | somar à linha de SaaS |
| Push notifications (Firebase) | R$ 0 até volumes altos |
| **TOTAL estimado** | custo Apple/Google + categoria acima |

---

## Regras VibeDev (aplicar SEMPRE antes do gate Fase 3 → 4)

### Regra 1 — Sempre apresente 3 portes
Ao escolher stack na Fase 3, apresente o custo estimado em **3 cenários**:
- "para os primeiros 1-10 usuários"
- "para 10-100 usuários"
- "para 100-1000 usuários"

Leigo nunca vai pensar no porte de cabeça. Você apresenta.

### Regra 2 — Marque "depende de uso"
Para serviços como API de IA, e-mail, pagamento, diga claramente:
"Custo cresce com uso. Não é fixo."

### Regra 3 — Domínio e SSL
Sempre que tem domínio, anote o custo anual. Tem gente que esquece.

### Regra 4 — LGPD pode ter custo
DPO, política de privacidade auditada, ferramentas de consentimento —
se for app com público brasileiro, isso entra na conta.

### Regra 5 — Kill criteria relacionado a custo
Se o `custo_mensal_estimado` no porte 10-100 já estoura o orçamento do
projeto, isso é sinal de voltar pra Fase 2 (Especificação) e cortar escopo.

### Regra 6 — Recusa educada de avançar
Se o usuário quiser pular a discussão de custo, diga:
"Para fechar a escolha de ferramentas, preciso te mostrar o custo
estimado de manter isso rodando. São 3 cenários, sem compromisso de
comprar agora. Pode ser?"

Se mesmo assim o usuário pular: **registre no Decision Log** com a
observação de que o custo foi pulado, e prossiga (decisão humana > regra).

---

## Aviso honesto sobre os números

Estes valores são **estimativas de ordem de grandeza** baseadas em serviços
comuns em 2025-2026. Não são cotação. Não incluem:
- Tempo humano de manutenção
- Custo de design / copy / imagem
- Impostos sobre serviço
- Custos eventuais de suporte ao cliente

Para cotar de verdade: entrar no site de cada serviço antes de fechar.

---

## Modelo de registro no `PROJECT_STATE.md`

```markdown
## Custo mensal estimado (registrado na Fase 3)

| Porte de usuários | Custo estimado (BRL/mês) |
|---|---|
| 1-10 (validação) | R$ XX |
| 10-100 (crescimento inicial) | R$ XXX |
| 100-1000 (operação) | R$ X.XXX |

**Decisão registrada em:** AAAA-MM-DD
**Premissas consideradas:** [lista de serviços e portes]
**Riscos de explosão de custo:** [ex: "volume de API de IA variável"]
**Quem paga:** [definição de quem banca isso]
```
