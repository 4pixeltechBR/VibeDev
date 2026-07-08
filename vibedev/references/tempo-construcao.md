# Estimativa de Tempo de Construção — Referência VibeDev

> Tabela de referência pra calibrar expectativa ANTES de começar um projeto.
> Carregada automaticamente no início do `/vd-start` em modo leigo.
> Apresentada como histórico de leigos anteriores — sem promessa.

---

## Por que isso existe

O leigo chega com a timeline mental de um reel de Instagram: "fiz um app em
20 minutos com IA". Quando encontra a realidade (dias, semanas, meses de
Fases 2-6), desiste na frustração.

Esta tabela serve pra **realinhar expectativa de tempo** antes de começar,
não depois. É dado de plano, sem compromisso firme — ritmo, contexto,
disponibilidade e escopo mudam tudo.

**Modo de falar (leigo):** "Vou te mostrar quanto tempo outros leigos
demoraram pra construir coisas parecidas com a sua ideia. Não é
promessa — é ponto de partida."

---

## Tabela por categoria

### 🟢 Landing page (1 página estática)
- **Sem IA, dev experiente:** 2-8 horas
- **Com IA, leigo na primeira vez:** 1-3 dias (incluindo ler, escolher stack, deploy)
- **Com IA, leigo na segunda vez:** 4-8 horas
- **O que leva tempo:** configurar domínio, decidir layout, escrever copy

### 🟢 Site institucional (3-10 páginas + blog opcional)
- **Sem IA, dev experiente:** 1-3 semanas
- **Com IA, leigo:** 2-4 semanas
- **Com IA, dev:** 3-7 dias
- **O que leva tempo:** organizar conteúdo, SEO básico, formulário de contato funcionando

### 🟢 App interno / dashboard privado (login + CRUD de uma coisa só)
- **Sem IA, dev:** 2-6 semanas
- **Com IA, leigo:** 1-2 meses
- **Com IA, dev:** 1-3 semanas
- **O que leva tempo:** autenticação, banco, deploy seguro, entender o que tá rodando

### 🟡 Bot em mensageria (Telegram/WhatsApp com LLM por trás)
- **Sem IA, dev:** 1-2 semanas
- **Com IA, leigo:** 3-8 semanas
- **Com IA, dev:** 1-2 semanas
- **O que leva tempo:** aprovação da API do canal, custo variável de API de IA, lidere com abuso/spam, prompts do LLM

### 🟡 SaaS B2C público (freemium, login, pagamento)
- **Sem IA, dev:** 2-6 meses
- **Com IA, leigo:** 3-12 meses
- **Com IA, dev:** 1-3 meses
- **O que leva tempo:** LGPD, pagamentos (Stripe/Pix), suporte ao usuário, manter servidor no ar, marketing

### 🟡 SaaS B2B (painel pra empresa cliente)
- **Sem IA, dev:** 2-4 meses
- **Com IA, leigo:** raramente vale — leigo quase sempre precisa de dev parceira
- **O que leva tempo:** SLA, contratos, integrações com sistemas do cliente, suporte

### 🔴 App mobile (iOS + Android na loja)
- **Sem IA, dev:** 3-9 meses
- **Com IA, leigo:** raramente termina em qualidade publicável — leigo muitas vezes trava aqui
- **O que leva tempo:** aprovação Apple/Google, push notifications, build pra iOS exige Mac, atualização constante pras lojas

### 🔴 Marketplace / two-sided (consumidor + prestador, ex: Uber, iFood)
- **Sem IA, dev:** 6-18 meses
- **Com IA, leigo:** raramente — é empresa de verdade
- **O que leva tempo:** ambos os lados precisam estar crescendo junto (problema da galinha e ovo)

### 🔴 App com IA generativa no centro (chatbot, gerador de imagem, etc)
- **Sem IA, dev:** 2-6 meses
- **Com IA, leigo:** 1-4 meses (MVP)
- **Com IA, dev:** 2-6 semanas (MVP)
- **O que leva tempo:** custo variável de API (esse é o real custo), moderação de conteúdo, latência

---

## Como essas estimativas funcionam (a verdade por trás)

As estimativas acima **somam todas as 8 Fases** do VibeDev. Algumas regras:

1. **Fase 1 (Validação)** — 1 sessão, 30min a 2h. Quase nunca o gargalo.
2. **Fase 2 (Especificação)** — 1-3 sessões. Vira gargalo pra leigo porque escrever o que quer é trabalho de verdade.
3. **Fase 3 (Arquitetura)** — 1-2 sessões. Decide stack + custo.
4. **Fase 4 (Segurança)** — embutida em Fase 6. Não conta como fase separada no leigo.
5. **Fase 5 (Versão & Estrutura)** — embutida. Conta como parte do setup.
6. **Fase 6 (Construção)** — **a Fase real, 70-90% do tempo**. Cada sub-tarefa: 🟢 15min / 🟡 1h / 🔴 2h+. Multiplica e soma.
7. **Fase 7 (Homologação)** — 1-3 sessões.
8. **Fase 8 (Operação)** — contínua depois do lançamento.

**Conclusão honesta:** se você tem 1 ideia, 1 fim de semana e zero experiência, dá pra entregar MVP de landing page ou bot simples. Tudo além disso é projeto de **mínimo 2-4 semanas**, com 2-4h por dia dedicado.

---

## Calibração por perfil (sem julgamento)

| Perfil | Multiplicador honesto |
|---|---|
| Dev com 5+ anos | x 1 (referência base) |
| Dev com 1-5 anos | x 1.3 |
| Leigo dedicado (4h+ por dia, foco) | x 2 a 3 |
| Leigo nas horas vagas (1-2h por dia) | x 3 a 5 |
| Leigo que tá aprendendo IA e Python junto | x 5 a 10 |

**Onde o leigo cai de cara:** assume multiplicador 1, na verdade é 3-5.

---

## Como apresentar (modo leigo)

Ao diagnosticar a categoria no `/vd-start`, **antes de dizer "vamos começar"**, fale:

> "Pra você calibrar: pessoas com ideia parecida com a sua geralmente levam X
> a Y semanas trabalhando Z horas por dia. Não é regra firme — é pra você
> não achar que tá atrasado quando não tá. Topa?"

Se a pessoa claramente não tem esse tempo, **mencione `/vd-kill` como opção
legítima**:

> "Se esse tempo não cabe na sua vida agora, isso não é fracasso —
> significa que essa ideia precisa amadurecer ou encontrar a hora certa.
> Posso te mostrar como arquivar com dignidade, ou a gente continua
> sabendo do ritmo?"

---

## Categorias ambíguas (pra ajudar a classificar)

Se a pessoa falou "quero fazer um app", geralmente é:

- **"app de presence no rolê com amigos"** → App interno + simples = 🟢
- **"app onde eu vendo meu produto"** → SaaS B2C simples = 🟡
- **"chatbot pra atender cliente no WhatsApp"** → Bot simples = 🟢
- **"app tipo iFood"** → Marketplace = 🔴 (provavelmente não é viável pra leigo solo)
- **"jogo mobile"** → Categoria totalmente diferente, fora do escopo VibeDev
- **"site da minha empresa"** → Site institucional = 🟢

Quando ambíguo, pergunte: **"Pra quem é isso? E o que essa pessoa consegue
fazer lá que antes não conseguia?"** Resposta define categoria em 80% dos casos.

---

## Aviso honesto final

Se o leigo chegou aqui querendo timeline de "1 fim de semana pro app estar
rodando e dando dinheiro", a chance de isso acontecer **sem partnership
técnica** é baixa. Isso não é pra desmotivar — é pra alinhar expectativa
pra que o projeto não morra na frustração do mês 2.

VibeDev é framework de **construção sólida**. Não é framework de "faça
dinheiro rápido com IA". A honestidade sobre tempo é parte do respeito
ao usuário.
