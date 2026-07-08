# Anti-Feature-Creep — Detecção e Contenção

> Mecanismo de defesa contra expansão não-declarada do escopo durante o `/vd-build`.
> Inspirado no anti-pattern "specification gaming" e "feature creep" da literatura
> de engenharia de software — e na regra universal do VibeDev:
> "ideias boas → backlog, código depois".

---

## O problema

Leigo (e dev também) tendem ainflar o escopo do `/vd-plan` durante o `/vd-build`:

- "Ah, posso colocar login com Google também?"
- "Já que tô mexendo aqui, dá pra adicionar notificação push?"
- "E se tivesse também histórico do usuário?"

Sem alguém dizendo "isso é Fase 2 (Especificação), não Fase 6 (Construção)"
o escopo cresce, o budget estoura, a frustração chega.

---

## Como o guard funciona (3 camadas)

### Camada 1 — Scope check pré-código (no `/vd-build`, antes de cada sub-tarefa)

Antes de escrever código de uma sub-tarefa, a IA compara a intenção com:

- **Anti-escopo** declarado na Fase 2 (`O que NÃO entra na v1`)
- **Critério de pronto** da sub-tarefa ativa
- **Lista de sub-tarefas restantes** do plano atual

Se a intenção da sub-tarefa não bate com nenhum desses três → **registra no
backlog do estado** e prossegue **só com o que tava planejado**.

### Camada 2 — Detecção em tempo real (durante a execução)

Se durante `/vd-build` surgir uma ideia nova (seja da IA, seja do usuário),
a IA deve:

1. **Reconhecer a ideia como boa** (1 frase, sem dismissar)
2. **Anotar no backlog** do `PROJECT_STATE.md` automaticamente
3. **Confirmar que continua na sub-tarefa atual, não na ideia nova**
4. **No fim da sub-tarefa**, mencionar o item adicionado ao backlog

Formato de entrada no backlog:

```markdown
- [yyyy-mm-dd] [SUB-TAREFA-X.Y] <descrição curta da ideia> — deferida para fase posterior
```

### Camada 3 — Bloqueio explícito em decisões de alto impacto

Certos tipos de ideia nova **não podem** ser adiadas pro backlog — precisam
de uma mini-decisão Tipo 1 antes de prosseguir:

- Adicionar autenticação (não estava no plano)
- Trocar banco de dados
- Adicionar pagamento / integração com serviço externo
- Adicionar armazenamento de dado pessoal novo
- Adicionar canal de comunicação novo

Nesses casos, **pause `/vd-build`** e chame uma mini-versão do `/vd-plan`:
"isso adiciona Y componente novo — quer a gente parar, atualizar o plano
com 3 opções, ou guardar no backlog pra Fase 7+?"

---

## Como apresentar ao usuário (modo leigo)

Quando a IA detecta feature creep, use tom neutro e factual:

> "Pera, isso aqui é uma ideia nova — não tava no que a gente combinou.
> Posso:
> 1) Anotar no caderno de tarefas pra fazer depois,
> 2) Parar agora e fazer um mini-plano de 5min pra essa ideia,
> 3) Ignorar.
> O que faz mais sentido pra você?"

Sem julgamento, sem urgência forçada. Decisão é do humano.

---

## Vícios comuns a evitar (pela IA)

| Comportamento errado | Por que evita |
|---|---|
| "Ah, boa ideia, vou fazer também" | Incha o escopo sem registro |
| "Isso é fora do escopo" (sem alternativa) | Soa como gatekeeper chato |
| "Vou colocar isso no backlog silenciosamente" | Backlog vira lixeira invisível |
| "Vou expandir o plano sozinho" | Tira autonomia do humano |

---

## Backlog honesto (o que NUNCA entra)

Backlog **NÃO** é saco de gatos. Itens que entram sem chance real de
implementação viram lixo emocional. Regra:

- Item entra no backlog **só se** for razoavelmente possível que um dia
  seja executado.
- Se for absurdamente grande ("refazer tudo em Rust") → fala "isso não cabe
  nem como backlog honesto — é projeto novo".
- Backlog é revisado **na transição de fase**, não mensalmente perdido.

---

## Relação com outras regras VibeDev

| Regra | Relação |
|---|---|
| Anti-escopo da Fase 2 | É a lista-mãe. Backlog complementa, não substitui. |
| Decision Log | Ideia que virou decisão Tipo 1 = Decision Log. Ideia deferida = backlog. |
| `/vd-kill` | Se backlog incha muito, é sinal de maturação do projeto. `/vd-kill` pode ser resposta. |
| `/vd-compact` | Backlog é candidato natural de compactação após fase fechada. |
| Glossário Ativo | Toda interação do anti-creep usa o glossário pro leigo. |

---

## Limites do guard

Esse guard **não substitui** autodisciplina do humano. Se o usuário quiser
mesmo fazer a feature nova agora, o guard registra e segue. A função dele é
**dar visibilidade** — sem visibilidade, scope creep é invisível até estourar.
