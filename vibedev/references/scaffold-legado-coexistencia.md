# Legado / Coexistência — Scaffold VibeDev

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Caso de borda Cynefin (Caótico/Complicado): sistema legado (WordPress, ERP,
   planilha) precisa continuar vivo junto com a aplicação nova. **Sempre
   dispara como decisão Tipo 1 de alta incerteza** — apresenta 3 opções, pergunta.

## 🇧🇷 Por que não está na tabela pré-validada

WordPress, ERPs (SAP, TOTVS, Omie), e planilhas como fonte de verdade são
**infinitamente diversos** — cada combinação (qual WP, qual versão, qual
plugin, qual ERP) é um caso único. Deixar como "stack pré-validada" faria o
framework parecer que **recomenda** WordPress — o que seria mentiroso e
anti-histórico (VibeDev é horizontal, agnóstico de stack).

Por isso: **3 opções são SEMPRE apresentadas ao usuário**, e a decisão é
**explicitamente registrada como Tipo 1**.

## 🇺🇸 Why not in the pre-validated table

WordPress, ERPs (SAP, TOTVS, Omie), and spreadsheets as source of truth are
**infinitely diverse** — each combination (which WP, which version, which
plugin, which ERP) is a unique case. Leaving it as "pre-validated stack" would
make the framework look like it **recommends** WordPress — which would be
dishonest and anti-historical (VibeDev is horizontal, stack-agnostic).

So: **3 options are ALWAYS presented to the user**, and the decision is
**explicitly recorded as Type 1**.

---

## 📋 As 3 opções (sempre)

### Opção 1: Manter legado como sistema de verdade, novo sistema lê dele

**Stack nova:** headless consumer do legado
- Se WordPress: **WordPress headless** (REST API ou GraphQL via WPGraphQL)
- Se ERP: conector que lê do ERP via API ou ODBC
- Se planilha: Google Sheets API ou similar

**Prós:**
- Zero risco de quebrar o legado
- Time aprende a stack nova gradualmente
- Legado continua sendo a "fonte da verdade" (regulatory-friendly)

**Contras:**
- Latência: novo sistema fica refém da performance do legado
- Schema do legado vira teto (não pode modelar o que o legado não tem)
- Dívida técnica cresce (2 sistemas pra manter)

**Indicado quando:**
- Legado é core do negócio (loja rodando, clientes pagando, regulação em cima)
- Time é pequeno e não pode reescrever tudo
- Migração de dados é arriscada demais (volume, qualidade)

---

### Opção 2: Migrar tudo pro novo, de legado pra plano de desligamento

**Stack nova:** independente, com migration única do legado
- Identifica dados críticos do legado
- Migration one-shot (ou em waves) pra nova stack
- Legado entra em **read-only** depois da migration
- Legado é desligado em data pré-acordada (90-180 dias pós-launch)

**Prós:**
- Sistema novo sem amarras
- Longo prazo: 1 sistema só, sem dívida
- Performance e modelagem livres

**Contras:**
- Risco alto de migration (perda de dados, downtime)
- Custo inicial alto (escrita da migration + plano de desligamento)
- Equipe precisa ser capaz de manter o novo sistema

**Indicado quando:**
- Legado está em fim de vida (sem update há 2+ anos)
- Time tem capacidade técnica pra migration
- Janela de manutenção aceitável (pode ter 2-6h de downtime)

---

### Opção 3: Sincronização bidirecional (legado + novo coexistem)

**Stack nova:** independente, com sincronização em tempo real ou batch
- Eventos do legado propagam pro novo (CDC — Change Data Capture)
- Eventos do novo propagam pro legado
- Resolver conflitos (legado ganha? novo ganha? merge?)

**Prós:**
- Operacionalmente flexível (cada sistema tem dono diferente)
- Rollback fácil (se novo falhar, legado é fonte da verdade)
- Migração gradual (times podem mover features aos poucos)

**Contras:**
- Complexidade alta (CDC, resolução de conflitos, observabilidade)
- Debugging é pesadelo (qual sistema é a verdade em cada momento?)
- Custo de infra (Debezium, Kafka, ou similar)

**Indicado quando:**
- Legado e novo têm donos/organizações diferentes
- Não há acordo sobre migração total (motivos políticos, regulatory, etc)
- Equipe tem senioridade pra manter 2 sistemas sincronizados

---

## 🚦 Como a Triagem detecta este caso

Na 3ª pergunta da Triagem padrão:

> 3. "Existe algum sistema legado (WordPress, ERP, planilha) que precisa
>    continuar vivo junto com isso?"

Se a resposta for **"sim, com restrição"** → caso de borda, apresentar as 3 opções acima.
Se a resposta for **"não"** → segue pra tabela de stacks pré-validadas.
Se a resposta for **"sim, mas pode ser desligado"** → cai na Opção 2 (migrar tudo).

---

## 🔧 Padrões de implementação por tipo de legado

### WordPress Headless

```typescript
// Frontend consome WP via GraphQL
import { gql, useQuery } from "@apollo/client";

const GET_POSTS = gql`
  query GetPosts {
    posts(first: 10) {
      nodes {
        id
        title
        slug
        excerpt
        date
        featuredImage {
          node {
            sourceUrl
          }
        }
      }
    }
  }
`;

function PostList() {
  const { data, loading, error } = useQuery(GET_POSTS);
  // ... renderiza posts do WP
}
```

**Plugin WP necessário:** WPGraphQL
**Auth:** JWT via WPGraphQL JWT Authentication
**Quando usar:** site de conteúdo que virou app, blog institucional, e-commerce simples

### ERP (SAP, TOTVS, Omie) — connector

```python
# Exemplo: lendo do TOTVS via API REST
import httpx

class TOTVSConnector:
    def __init__(self, base_url: str, client_id: str, client_secret: str):
        self.base_url = base_url
        self.client_id = client_id
        self.client_secret = client_secret
        self._token = None

    async def authenticate(self):
        async with httpx.AsyncClient() as client:
            r = await client.post(
                f"{self.base_url}/api/oauth2/token",
                data={
                    "grant_type": "client_credentials",
                    "client_id": self.client_id,
                    "client_secret": self.client_secret,
                },
            )
            r.raise_for_status()
            self._token = r.json()["access_token"]

    async def get_clientes(self, limit: int = 100):
        if not self._token:
            await self.authenticate()
        async with httpx.AsyncClient() as client:
            r = await client.get(
                f"{self.base_url}/api/v1/clientes",
                params={"limit": limit},
                headers={"Authorization": f"Bearer {self._token}"},
            )
            r.raise_for_status()
            return r.json()["items"]
```

**Quando usar:** ERP é fonte de verdade pra clientes, produtos, financeiro

### Planilha (Google Sheets) como banco

```python
# Sync diário: Google Sheets → Postgres
import gspread
from google.oauth2.service_account import Credentials

async def sync_sheet_to_db(sheet_id: str, table: str, db: SessionDep):
    creds = Credentials.from_service_account_file("gsheets-creds.json")
    client = gspread.authorize(creds)
    sheet = client.open_by_key(sheet_id).sheet1
    records = sheet.get_all_records()

    for record in records:
        # UPSERT na tabela
        stmt = pg.insert(table).values(record)
        stmt = stmt.on_conflict_do_update(
            index_elements=["external_id"],
            set_=record,
        )
        await db.execute(stmt)
    await db.commit()
```

**Quando usar:** legado é planilha que virou "mini-ERP" (muito comum em PMEs BR)

---

## 🔍 Checklist de validação (qualquer opção escolhida)

- [ ] Decisão registrada como Tipo 1 de alta incerteza
- [ ] Red Team aplicado: "o que invalida essa escolha em 12 meses?"
- [ ] Plano de saída do legado documentado (quando e como desligar)
- [ ] Se Opção 1 (headless): latência documentada e SLA combinado
- [ ] Se Opção 2 (migrar): plano de migration com rollback testado
- [ ] Se Opção 3 (sync): resolução de conflitos definida (legado ganha? novo? merge?)
- [ ] Equipe técnica tem senioridade pra opção escolhida
- [ ] Compliance/LGPD: dados migrados mantém proveniência
- [ ] Logs de sincronização (pra debug quando "sumiu um registro")

---

## ⚠️ Anti-padrões (bloqueados pelo `/vd-check`)

- ❌ WordPress como **única** stack recomendada
- ❌ Decisão de coexistência tomada sem Red Team
- ❌ Opção 3 (sync bidirecional) sem estratégia de resolução de conflitos
- ❌ Migration sem plano de rollback
- ❌ Legado desligado sem plano de read-only prévio
- ❌ Sincronização unidirecional sem log de eventos (debugging impossível)
- ❌ Connector de ERP sem rate limiting (ERP cai sob carga)

---

## 🔗 Ver também

- `scaffold-stack-validada.md` — stacks padrão (sem legado)
- `scaffold-schema-migrations.md` — schema da stack nova
- `references/trilha-vermelha.md` — resgate de projeto legado (outro ângulo)
- `/vd-arch-review` — audita integração com legado depois de pronto
- `/vd-check` — gate que valida este artefato

---

## 📚 Inspirações

- **Cynefin Framework** (Dave Snowden, 1999) — categoria Caótico/Complicado
- **WordPress Headless** (wpgraphql.com) — base da Opção 1 com WP
- **Strangler Fig Pattern** (Martin Fowler, 2004) — base da Opção 2
- **Change Data Capture** (Debezium, 2015+) — base da Opção 3
- **Dante Testa — Prompt Mestre WP v1.14.0** (jul/2026) — caso de borda WP
