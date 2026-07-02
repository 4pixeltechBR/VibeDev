# Stack Guide — Recomendações por Caso de Uso

Este arquivo é consultado durante a Fase 3 (Arquitetura) da Trilha Verde
ou durante a R2 (Triagem) da Trilha Vermelha para avaliar a stack existente.

As recomendações são padrão para vibe coders sem experiência prévia.
Sempre documente a escolha final como Tipo 1 no Decision Log.

---

## Critérios de seleção

Antes de recomendar uma stack, confirme:
1. **Qual linguagem o usuário já conhece minimamente?** (se nenhuma, Python)
2. **Precisa de banco de dados?** (sim para quase tudo que persiste dados)
3. **Vai ter usuários externos ou é uso próprio?** (muda requisitos de auth e deploy)
4. **Qual o orçamento de hospedagem?** (zero → soluções gratuitas; pago → mais opções)
5. **Precisa rodar localmente ou na nuvem?**

---

## Casos de uso comuns

### App web com usuários (SaaS, ferramenta interna, MVP)
**Recomendado:** Python + FastAPI + PostgreSQL + Railway/Render
- FastAPI: simples, rápido, documentação automática
- PostgreSQL: robusto, gratuito, suportado em todo lugar
- Railway/Render: deploy em minutos, plano gratuito disponível
- Auth: use biblioteca pronta (FastAPI-Users, Auth0 gratuito até certo volume)

**Alternativa mais simples:** Python + Flask + SQLite (se volume baixo)
**Alternativa JavaScript:** Next.js + Supabase (banco + auth prontos)

### Bot para Telegram / WhatsApp
**Recomendado:** Python + python-telegram-bot ou whatsapp-web.py + SQLite
- SQLite: suficiente para bots com poucos usuários simultâneos
- Rodar em VPS barato (Hetzner ~€4/mês) ou Railway

**Cuidado:** WhatsApp não tem API oficial gratuita — soluções não-oficiais
podem ser bloqueadas. Avalie o risco antes (Tipo 1).

### Agente de IA / Automação com LLM
**Recomendado:** Python + LangChain ou LangGraph + SQLite + Ollama (local)
- Ollama: rodar modelos localmente, zero custo de API
- SQLite com WAL: suficiente para agentes com memória
- Se precisar de API externa: OpenAI, Anthropic, Groq (free tier)

### Site / Landing page / Blog
**Recomendado:** HTML + CSS + JavaScript puro (sem framework)
- Deploy: Vercel, Netlify, GitHub Pages — todos gratuitos
- Se precisar de CMS: Notion como backend + API pública

**Alternativa com mais poder:** Next.js (se precisar de server-side)

### Automação / Script pessoal
**Recomendado:** Python puro + SQLite se precisar persistir
- Sem servidor, sem deploy, roda local
- Dependências mínimas

### Dashboard / Análise de dados
**Recomendado:** Python + Streamlit + Pandas
- Streamlit: interface web em Python puro, sem HTML/CSS
- Deploy gratuito no Streamlit Cloud

### API para integrar sistemas
**Recomendado:** Python + FastAPI + PostgreSQL
- FastAPI gera documentação automática (Swagger)
- Fácil de testar com Insomnia ou Postman

---

## Bancos de dados — quando usar qual

| Banco | Quando usar | Quando NÃO usar |
|-------|-------------|-----------------|
| SQLite | Scripts, bots, apps com < 100 usuários simultâneos, local-first | Múltiplos processos escrevendo ao mesmo tempo, produção com muito volume |
| PostgreSQL | Apps web, SaaS, qualquer coisa com usuários externos | Você quer evitar configuração — use Supabase/Railway que gerenciam pra você |
| MongoDB | Dados muito variáveis sem estrutura definida | Se você não sabe exatamente por que quer NoSQL — padrão é SQL |
| Redis | Cache, filas, dados temporários | Armazenamento principal de dados |

---

## Hospedagem — opções por orçamento

| Orçamento | Opção | Para quê |
|-----------|-------|---------|
| Zero | Railway free tier, Render free, Vercel, Netlify | MVP, teste, projetos pessoais |
| ~R$20/mês | Railway starter, Render starter | Apps pequenos em produção |
| ~R$25/mês | Hetzner CX22 (VPS) | Controle total, bom custo-benefício |
| ~R$100/mês | DigitalOcean Droplet + banco gerenciado | Apps médios com SLA |

**Recomendação para vibe coders iniciando:** Railway ou Render — deploy
automático via git, sem configurar servidor, plano gratuito suficiente
para MVP.

---

## Red flags de stack a evitar para iniciantes

- **Kubernetes / Docker Swarm:** complexidade desnecessária para < 10k usuários
- **Microserviços:** comece monolítico, quebre depois se necessário
- **Banco de dados exótico (Cassandra, CouchDB):** sem razão clara = dívida técnica
- **Framework desconhecido só porque é "moderno":** custo de aprendizado real
- **Três ORMs diferentes no mesmo projeto:** Frankenstein de dados

---

## Avaliando uma stack existente (Trilha Vermelha)

Perguntas para a Fase R2:
1. A stack escolhida resolve o problema ou foi escolhida por moda?
2. O usuário consegue manter e debugar sozinho?
3. Existem dependências abandonadas ou sem manutenção ativa?
4. A stack tem documentação adequada em português ou inglês acessível?
5. O custo de operação está dentro do kill criteria?

Stack ruim mas funcional: documente no Decision Log como dívida técnica
conhecida. Não refatore antes de estabilizar (R3) e remediar P0s (R4).
