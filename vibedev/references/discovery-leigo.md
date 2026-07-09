# Discovery Leigo — Como ajudar a transformar ideia vaga em projeto real

> Framework de conversa pra quando o usuário chega com uma ideia solta,
> sem saber ainda o que precisa decidir. Roda ANTES do `/vd-start` formal.
> Pode ser ativado por:
> - Comando explícito: `/vd-spark`
> - Detecção automática quando o projeto é novo e a intenção do usuário
>   é ambígua (< 3 frases ou sem "quem usa" / "o que resolve" / "como saber que deu certo").

---

## Por que isso existe

O `/vd-start` formal pede 3 perguntas diagnósticas densas (código existente?
está em produção? descreva o projeto em 1-2 frases). Para quem tem uma
ideia recém-vista num reel de Instagram, essas perguntas travam.

`/vd-spark` é a porta de entrada suave: conduz a conversa em rodadas curtas
pra extrair o que precisa antes do formal, **sem mencionar framework**.

---

## As 4 rodadas de `/vd-spark`

### Rodada 1 — A ideia (1 frase)

Pergunte só uma coisa:

> "Me conta em 1 frase: o que você tá querendo construir?"

Não peça pra estruturar. Não peça lista de features. Só a essência.

**Sinal verde pra avançar:** pessoa conseguiu falar em 1 frase (mesmo que bagunçada).

**Sinal de retorno:** "não sei nem resumir" / "tenho várias ideias" → pergunte qual delas tá mais acesa hoje. Forçar a escolher 1.

### Rodada 2 — Pra quem

> "Quem é a pessoa que vai usar isso? Pode ser você mesmo, um amigo específico, um tipo de cliente — qualquer um vale."

Por que essa pergunta é poderosa: leigos costumam pensar primeiro em "o que
tecnologia faz" em vez de "quem serve". Essa rodada inverte.

Se a resposta for vaga ("todo mundo", "qualquer pessoa"), refinar:

> "Se só UMA pessoa pudesse usar isso e ser muito feliz com ele, quem seria?"

**Sinal verde:** pessoa consegue descrever 1 perfil (ainda que seja "eu mesmo").

### Rodada 3 — O que muda pra essa pessoa

> "O que essa pessoa consegue fazer DEPOIS de usar isso, que antes não conseguia?"

Por que essa pergunta é poderosa: força descrever **transformação**, não
**funcionalidade**. Diferença sutil mas crítica.

**Resposta fraca:** "vai ter login", "vai ser bonito", "vai usar IA" — isso é o que o app FAZ, não o que MUDA.

**Resposta forte:** "vai conseguir acompanhar as contas semanais sem planilha", "vai conseguir avisar os amigos do rolê em 1 clique", "vai conseguir comer sem passar vergonha em restaurante japonês".

Se a resposta for fraca, reformule com a pessoa:

> "Tá, e o que a pessoa PODE FAZIR com isso? Tipo uma ação concreta que ela faz depois de abrir o app?"

**Sinal verde:** descreve 1 ação concreta observável.

### Rodada 4 — Como vai saber que deu certo

> "Como você vai saber que essa ideia deu certo? Tipo, qual sinal concreto te faria pensar 'funcionou'?"

**Resposta fraca:** "vai bombar", "vai dar dinheiro", "vai viralizar".

**Resposta forte:** "10 pessoas vão usar pelo menos 1x por semana por 1 mês", "vou conseguir cobrar R$ 30/mês de 5 clientes", "minha mãe vai conseguir usar sem me ligar pra pedir ajuda".

**Sinal verde:** métrica observável + prazo implícito ou explícito.

### (Bônus, se sobrar tempo) — Anti-escopo

> "Tem alguma coisa que você JÁ sabe que NÃO vai entrar na primeira versão?"

Leigos raramente têm essa resposta pronta. Se não tiver, não force — só
deixa plantada pra depois.

---

## Quando transicionar pra `/vd-start` formal

Após as 4 rodadas, peça confirmação:

> "Beleza, já tenho o suficiente pra começar. Posso usar o que você falou
> pra montar o início do projeto? Vou te perguntar agora 2 coisinhas
> rapidinhas só pra confirmar — topa?"

Se sim → execute o `/vd-start` formal (bootstrap), usando as respostas do
`/vd-spark` como insumo pras 3 perguntas de diagnóstico.

Se não → fique mais 1 rodada conversando. Não pule.

---

## Comportamento da IA durante o `/vd-spark`

| Faça | Não faça |
|---|---|
| Pergunte uma coisa por vez | Não lance várias perguntas juntas |
| Reformule respostas vagas sem julgamento | Não diga "tá errado, melhor pensar de novo" |
| Repita o que entendeu pra confirmar | Não assuma que entendeu antes de confirmar |
| Mantenha tom leve e curioso | Não soe como entrevista de emprego |
| Use o glossário se modo leigo | Não use palavras como "MVP", "escopo", "feature" |
| Dê exemplos concretos na pergunta | Não fique só na abstração |

---

## Limites do `/vd-spark`

- Não toma decisões. Só extrai.
- Não escreve código. Só conversa.
- Não fala de stack, custo, modelo de dados, etc. Quem cuida disso é o `/vd-start` + `/vd-plan`.
- Não substitui o `/vd-start` formal. É **pré-`/vd-start`**.

---

## Output esperado

Ao final do `/vd-spark`, registre **no estado ou num rascunho** (campo `discovery_brief` em `PROJECT_STATE.md`) as 4 respostas:

```markdown
## Discovery Brief (do /vd-spark)

- **Ideia:** [resposta da Rodada 1]
- **Pra quem:** [resposta da Rodada 2]
- **O que muda pra essa pessoa:** [resposta da Rodada 3]
- **Como vou saber que deu certo:** [resposta da Rodada 4]
- **Anti-escopo declarado:** [resposta bônus, ou "nenhum declarado"]
- **Data:** AAAA-MM-DD
```

Esse bloco **alimenta o `/vd-start` formal** automaticamente — as 3 perguntas
de diagnóstico ficam mastigadas.

---

## Quando **NÃO** usar `/vd-spark`

- Usuário já chega com escopo claro (lista de features, persona, métrica)
  → vai direto pro `/vd-start`.
- Usuário é dev experiente → pula, vai pro bootstrap técnico.
- Usuário tá retomando projeto existente → vai direto pro `/vd-status`.

Detecção automática: se a primeira mensagem do usuário já tem "pra
[persona], [transformação observável], com medição [métrica]" — pula o
`/vd-spark` e roda o `/vd-start` direto. Sem fricção desnecessária.
