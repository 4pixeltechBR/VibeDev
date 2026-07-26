# /vd-devils-advocate

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Sub-comando do `/vd-plan`. Roda um pré-mortem na decisão ANTES de quebrar
> em sub-tarefas. Força o agente (e o usuário) a enxergar o que pode dar errado.

## 🇧🇷 O que faz

Ativado automaticamente como **gate** dentro do `/vd-plan` (opt-out via flag `--skip-devils-advocate`).

Lê o estado atual (`PROJECT_STATE.md` + `ARCH_MAP.md` se existir + a decisão que vai ser tomada) e gera 3 entregas obrigatórias:

1. **Top 3 riscos** da decisão (probabilidade × impacto, com explicação)
2. **1 contradição interna** que o agente detectou entre o que o usuário quer e o que o estado do projeto diz
3. **1 ângulo cego** — uma coisa que ninguém (nem o agente, nem o usuário) mencionou, mas que pode implodir o plano

O agente **NÃO pode prosseguir** sem que o usuário responda explicitamente a cada item (aceitar, mitigar ou descartar).

## 🇺🇸 What it does

Auto-activated as a **gate** inside `/vd-plan` (opt-out via `--skip-devils-advocate` flag).

Reads current state (`PROJECT_STATE.md` + `ARCH_MAP.md` if exists + the decision about to be made) and produces 3 mandatory outputs:

1. **Top 3 risks** of the decision (probability × impact, with explanation)
2. **1 internal contradiction** the agent detected between what the user wants and what project state says
3. **1 blind angle** — something nobody (not the agent, not the user) mentioned that could implode the plan

Agent **CANNOT proceed** without user explicitly responding to each item (accept, mitigate, or discard).

---

## 📋 Output format (append no PROJECT_STATE.md)

```markdown
## Devils Advocate — AAAA-MM-DD HH:MM
<!-- Gerado por /vd-plan como gate. Não prosseguir sem resposta do usuário. -->

### Top 3 riscos
1. **[Risco]** — prob: [baixa|média|alta] · impacto: [baixo|médio|alto] · por quê: [explicação]
2. **[Risco]** — prob: [...] · impacto: [...] · por quê: [...]
3. **[Risco]** — prob: [...] · impacto: [...] · por quê: [...]

### Contradição interna detectada
- **Tensão:** [o que o usuário disse em X vs o que PROJECT_STATE diz em Y]
- **Resolução proposta:** [sugestão do agente]

### Ângulo cego
- **[Cenário que ninguém pensou]:** [descrição curta]
- **Por que pode ser foda:** [explicação]

### Resposta do usuário
<!-- Obrigatório para /vd-plan prosseguir. -->
- [ ] Risco 1: [aceito|mitigo|descarto] — [comentário]
- [ ] Risco 2: [aceito|mitigo|descarto] — [comentário]
- [ ] Risco 3: [aceito|mitigo|descarto] — [comentário]
- [ ] Contradição: [resolução escolhida]
- [ ] Ângulo cego: [incorporo|descarto] — [comentário]
```

---

## 🚦 Comportamento do gate

| Situação | Ação |
|----------|------|
| Usuário rodou `/vd-plan` sem flag | Roda `/vd-devils-advocate` automaticamente |
| Usuário respondeu os 5 itens | Prossegue com sub-tarefas |
| Usuário digitou "skip" ou similar | Pede confirmação explícita; se confirmado, prossegue com aviso no log |
| Usuário não respondeu e fechou sessão | `/vd-close` registra como "DA pendente" no resumo |
| Decisão é Trivial (1 linha, sem impacto) | Auto-skip com aviso ("decisão trivial, gate pulado") |
| Projeto está em modo leigo | Usa linguagem simples, sem jargão de "prob × impacto" |

---

## 🛠️ Quando o `/vd-devils-advocate` falha (auto-fallback)

- **Não consegue ler `PROJECT_STATE.md`** → pergunta ao usuário em vez de inventar
- **Detecta mais de 3 riscos** → mostra os 3 mais graves, menciona "outros X no log"
- **Detecta mais de 1 contradição** → mostra a mais grave
- **Não encontra ângulo cego** → admite honestamente e segue (não inventa)

---

## 🧪 Exemplo (resumido)

```
> /vd-plan trocar SQLite por PostgreSQL

🤖 Vou rodar o pré-mortem antes de quebrar em sub-tarefas.

DEVILS ADVOCATE — 2026-07-26 01:55:00

Top 3 riscos:
1. Migração de dados sem backup — prob: média · impacto: alto · se SQLite
   já tem dados de produção, perder eles é irreversível
2. Overhead de Postgres para app pequeno — prob: alta · impacto: baixo ·
   se app é hobby/prototípico, Postgres é canhão pra matar mosca
3. Custo de tempo de migração não estimado — prob: alta · impacto: médio ·
   sub-tarefa "escrever script de migração" não foi quebrada ainda

Contradição interna: PROJECT_STATE.md diz "Protótipo pessoal" mas você tá
escolhendo Postgres. Postgres faz sentido pra SaaS com múltiplos clientes,
não pra protótipo pessoal. Quer refazer a constraint?

Ângulo cego: ninguém pensou em lock-in de fornecedor. Se for usar
Supabase/Neon (Postgres gerenciado), mudar de provedor é trivial. Se for
self-hosted, é 5-10h de setup de replicação/backup. Decida isso ANTES
de escolher a engine.

Responda os 5 itens ou digite "skip" pra pular (vai pro log).
```

---

## 🔗 Ver também

- `/vd-plan` — comando que invoca este sub-comando
- `/vd-check` — gate posterior, valida artefato (não decisão)
- `references/glossario-leigo.md` — termos em modo leigo
- `references/autoria-arquivos.md` — assinatura (independente)

---

## 📚 Inspirações

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026): "validação por advogado do diabo que faz pré-mortem dos riscos"
- **Gary Klein — "Performing a Project Premortem"** (HBR, 2007): técnica de imaginação de falha retroativa
- **Atul Gawande — "The Checklist Manifesto"** (2009): valor de checks simples antes de ação
- **Sandeco Macedo — arXiv:2607.00038** (jul/2026): "validação multi-camada em sistemas de IA" (citado em CREDITS.md)
