# Coleta Assistida de Features — `/vd-plan` sem Travar o Leigo

> Substituir o pedido aberto "liste 10 features testáveis" por conversa
> guiada que produz a mesma lista sem o leigo travar.

---

## Por que essa regra existe

Pedir "liste 10 features testáveis" pra um leigo:
1. Não sabe o que é feature
2. Não entende "testável"  
3. Trava
4. Lista 23 coisas aleatórias
5. Se frustra
6. Pede ajuda ou desiste

Coleta assistida reformula como conversa puxada por **blocos do fluxo principal**.

---

## A conversa substituta

Em vez do prompt fechado, IA faz 3 perguntas sequenciais:

### Pergunta 1 — Fluxo principal da pessoa

> "Me conta como você imagina que alguém vai usar o produto — passo a passo,
> do começo até o valor. Exemplo: pra um app de pedido de comida,
> seria: 'abre o app, procura restaurante, escolhe comida, paga, espera, recebe'."

Por que essa pergunta é poderosa:
- Leigo verbaliza, não enumera
- Sequência mostra **dependências** (não dá pra "pagar" antes de "escolher")
- IA consegue extrair features implícitas da narrativa

IA extrai: "criar conta", "buscar", "selecionar item", "pagar", "ver status do pedido", "avaliar"

### Pergunta 2 — Coisas que dão medo ou irritam a pessoa

> "Tem alguma coisa que você não quer que aconteça? Ou que tá acontecendo
> hoje no concorrente e te dá raiva?"

Exemplos:
- "não quero que cliente veja pedido de outro"
- "pagos duplicados me dão raiva"
- "atraso sem aviso é o pior"

IA extrai: "autenticação", "validação de pagamento idempotente", "notificação de status"

### Pergunta 3 — O que te faria dizer "isso aqui não é pra mim"

> "Tem algum cenário que você imagina o usuário pensando 'isso não é pra
> mim, vou embora'? Se você descrever isso, fica fácil saber o que tirar
> da v1."

IA extrai: além das features, ajuda a preencher **anti-escopo** automaticamente.

---

## Síntese

Após as 3 perguntas, IA:

1. Lista as features inferidas (umas 8-15)
2. Pergunta: "Falta alguma coisa importante?" → adiciona se usuário falar
3. Marca **MVP** (essencial) vs **Pós-MVP** (proxima versão) vs **não entra**
4. Para cada uma, escreve o critério de "testável" em linguagem humana:

> "Criar conta — testável quando você consegue preencher e-mail, senha,
> clicar em cadastrar, e ver mensagem 'conta criada, vai pro login'."

5. Apresenta a lista final. Usuário edita / remove / reordena.

---

## Critério "testável" explicado ao leigo

Antes da pergunta 1, IA dá 1 exemplo concreto do que entende:

> "Quando eu pergunto 'testável', quero dizer: 'a gente sabe que tá
> funcionando quando [isso aparece na tela / isso acontece na vida real]'.
> Exemplo: 'login é testável quando você consegue entrar com e-mail e senha
> e ver seu nome no canto da tela'. Não é testável dizer 'login bom'."

---

## Fallback "cole tudo num brainstorm"

Se leigo insiste em jogar todas as ideias de uma vez, IA captura tudo, depois **organiza por IA** nas 3 dimensões (fluxo / não-quero / anti-escopo) e devolve organizado.

"Se você tá com a cabeça cheia, joga tudo aqui e eu organizo depois.
Sem problema."

---

## Integração com `/vd-plan` formal

O `/vd-plan` formal continua exigindo:
- Features ordenadas do MVP (max 10)
- Anti-escopo
- Fluxo principal escrito

A coleta assistida **alimenta** esses campos. Quando o leigo termina a
coleta, o `/vd-plan` formal já tem 80% do material mastigado — só
confirma e refina.

---

## Aplicação automática em modo leigo

Quando `modo_usuario: leigo`:
- `/vd-plan` abre com coleta assistida automaticamente
- Não chama "MVP" / "testável" sem explicar antes
- Usa os blocos de perguntas acima em sequência

Quando `modo_usuario: tecnico` ou vazio:
- Comportamento padrão mantido (lista aberta de features)

---

## Outputs pra gravar no estado

Após coleta, IA preenche no `PROJECT_STATE.md`:

```markdown
## Features inferidas (coleta assistida em /vd-plan)

### MVP (ordem de prioridade)
1. [nome] — testável quando [observável]
2. ...

### Pós-MVP (próxima versão)
- ...

### Anti-escopo (não entra na v1)
- ...

### Origem
- Coletado em AAAA-MM-DD via /vd-plan assistido
- Persona: [nome do discovery_brief]
- Perguntas aplicadas: fluxo / não-quero / anti-escopo
```

---

## Quando parar / desistir

Se após 2 ciclos de coleta o leigo não consegue articular nada:
- Pode ser que a ideia ainda não tá madura
- Sugerir `/vd-kill` preventivo ou "esperar 1 semana e voltar"
- Sem julgamento, sem pressa

Forçar um leigo travado é pior que respeitar o silêncio dele.
