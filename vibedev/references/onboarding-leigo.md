# Onboarding Tour — Primeira Visita do Leigo

> Tour automático de 3 mensagens curtas mostrado **só na primeira sessão**
> de um projeto, **só** quando `modo_usuario: leigo`. Não é tutorial:
> é reconhecimento. O leigo precisa sentir que tem alguém explicando
> sem ser didático.

---

## Detecção da "primeira vez"

Sinal:
- `PROJECT_STATE.md` não existe ainda, OU
- existe mas não tem campo `onboarding_completo: true` no bloco de Identidade

Se um dos dois → roda o tour no `/vd-start`, no `/vd-status` inicial, ou no
primeiro pedido de help do usuário.

---

## Mensagem 1 — Reconhecimento honesto

> "Oi! Você chegou com uma ideia — e eu não vou fingir que a gente já sabe
> como ela vai virar app. Isso aqui (VibeDev) é um jeito de construir em
> etapas pequenas, com você decidindo o que importa em cada uma. Sem
> promessa de prazo."
>
> "Topa fazer 3 coisinhas comigo pra eu entender melhor o que você tá
> querendo? Leva uns 5 minutos."

**Tom:** humilde, claro, sem entusiasmo forçado. Sem "vou te ajudar a
realizar seu sonho".

---

## Mensagem 2 — Mapa visual do caminho

> "A ideia vai passar por aqui (5 etapas, do desenho ao lançamento):"
>
> ```
> [1 Entender o problema]
>        │
>        ▼
> [2 Decidir como construir]
>        │
>        ▼
> [3 Construir (essa é a maior)]
>        │
>        ▼
> [4 Testar]
>        │
>        ▼
> [5 Colocar no ar]
> ```
>
> "Você só precisa decidir em cada etapa — eu mostro as opções e você escolhe.
> Quando achar que travou, fala 'tô perdido' e eu te trago de volta pra onde a gente estava."

**Por que essa mensagem:** leigos não sabem que existe fases. Mostrar o mapa
reduz ansiedade de "será que vai demorar" e dá sensação de progresso.

---

## Mensagem 3 — Uma escolha binária pra começar

> "Pra gente começar, preciso saber uma coisa só agora:"
>
> "Você tá começando do ZERO (nada escrito) ou tem código que tá uma bagunça
> e você quer consertar?"
>
> "Zero → a gente desenha a ideia primeiro.
> Bagunça → a gente faz um raio-x de onde tá o problema antes."

**Por que binária:** leigo trava com leque aberto. Duas opções claras,
cada uma com consequência visível. Sem info-dumping.

---

## Após o tour

Marca no `PROJECT_STATE.md`:

```markdown
- **onboarding_completo:** true
- **data_primeira_sessão:** AAAA-MM-DD
```

A partir daqui, **tour não roda mais** na mesma sessão nem em sessões futuras.

---

## Como decidir se o tour roda em sessões futuras

Roda só se:
1. Projeto novo (`/vd-start` foi rodado agora)
2. **OU** o usuário tá sumido há 60+ dias e volta (`/vd-status` detecta)

Em qualquer outro caso (`/vd-status` numa sessão regular, perguntas soltas),
o tour **não dispara**.

---

## Personalização por modo país

Quando `modo-pais` estiver ativo:
- Tour inteiro no idioma local
- Fuso nos exemplos de timestamp
- Moeda local nos exemplos (se mencionar custo)
- Cultura local: leigo chinês pode preferir tom mais formal; leigo BR mais
  direto e informal. A IA calibra pelo histórico do usuário.

---

## Quando o tour **NÃO** roda

- Dev experiente (`modo_usuario: tecnico`)
- Projeto já tem onboarding_completo: true
- Usuário chega com escopo técnico claro, sem ambigüidade
- Usuário expressamente fala "não precisa explicar, só faz"
- Modo técnico: tour não roda em hipótese alguma

---

## Anti-patterns do tour

| Comportamento errado | Por que |
|---|---|
| 10 telas de tutorial | Não é produto de software, é conversa |
| Mensagem única gigante | Leigo não lê parede de texto |
| Tour repetindo em cada sessão | Atrapalha, não ajuda |
| Forçar ordem rígida | Leigo pode ter pulado etapas — respeitar |
| Tom professoral ("antes de tudo, você precisa entender...") | Imediatamente desiste |
