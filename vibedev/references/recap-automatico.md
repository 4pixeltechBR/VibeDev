# Recap Automático — Quando o Leigo Volta Depois de Dias Sumido

> O leigo larga o projeto uns dias. Volta sem lembrar onde parou.
> Sem recap, a primeira sessão é perdida relendo estado. Com recap,
> o framework lê, resume em linguagem humana, e pergunta: continua ou
> quer revisar antes?

---

## Quando dispara

`/vd-status` detecta automaticamente:

- `ultima_sessao_em` campo no `PROJECT_STATE.md` (atualizado pelo `/vd-close`)
- Data atual - data última sessão ≥ 7 dias

Quando dispara, em vez do `/vd-status` normal, o framework entrega
**recap estendido** + pergunta se quer continuar ou revisar.

---

## Render do recap estendido

```
═══════════════════════════════════════════
   BEM-VINDO DE VOLTA
═══════════════════════════════════════════

Você sumiu há 12 dias. Última vez foi em 26/06/2026.
Eu não tive como saber se você desistiu, travou, ou tava só ocupado.

Aqui é onde você estava:

   Fase 3 — Construir com cuidado
   Sub-tarefa: 3 de 8 — "tela de login"

O que tinha ficado pronto antes de você sumir:
   ✅ 28/06 — modelo de usuário no banco
   ✅ 29/06 — função de login no backend
   ✅ 01/07 — testes da função de login passando
   ➔ (parou aqui, 03/07)

Decisões importantes que você tomou (e que eu não vou mudar sem você pedir):
   • Stack: Next.js + Postgres + Supabase Auth
   • Hash de senha: bcrypt rounds=12
   • Idioma da UI: pt-BR
   • Moeda mostrada: BRL

O que mudou no projeto enquanto você tava fora:
   • Nada — você é quem mexe nele.

Sua ideia original (do /vd-spark):
   "App pra organizar as contas do casamento, eu e minha noiva.
    Pra quando a gente parar de discutir dinheiro."

═══════════════════════════════════════════
   1) Continua de onde parou (tela de login)
   2) Revisa o que tem pronto antes (reler /vd-check anterior)
   3) Mudou o plano — você me explica
═══════════════════════════════════════════
```

---

## Comportamento pós-pergunta

Se resposta for "1" → continua, vai pra próxima sub-tarefa com `/vd-build`.
Se resposta for "2" → IA pede descrição do que foi testado, recomeça validação.
Se resposta for "3" → IA pergunta "o que mudou", registra no estado, recalcula plano.

---

## Quando **não** dispara

- Volta em ≤ 7 dias: `/vd-status` normal, painel visual.
- Projeto novo: tour de onboarding, não recap.
- Volta depois de 90+ dias: `/vd-spark` é sugerido (redescobrir a ideia pode
  ter mudado nesse tempo).

---

## Detecção de sumiço real (vs. ocupado)

O framework **não sabe distinguir** entre "tava ocupado" e "desisti silenciosamente". Não tenta adivinhar. Apenas:

- Pergunta educadamente se quer continuar (não assume)
- Apresenta o que tem
- Não julga o silêncio
- Oferece `/vd-kill` como saída se a ideia já não fizer sentido

---

## Atualização do campo `ultima_sessao_em`

O `/vd-close` é o único momento que mexe nesse campo. Formato:

```markdown
- **ultima_sessao_em:** AAAA-MM-DD
```

Sempre AAAA-MM-DD, hora não importa (leigo não vai operar nesse nível).

---

## Limites do recap

- **Não tenta "lembrar" coisa que não tá no estado.** Se o leigo não usou
  `/vd-close`, recap não tem info confiável. Aí o framework declara: "Eu não
  sei onde você tava. O que você lembra?"
- **Não inventa atividade.** Se o estado diz "última conquista: 01/07",
  o recap fala isso — não inventa nada entre 01/07 e hoje.
- **Respeita a porta.** Se o leigo responde "ah, deixa quieto, eu volto
  depois", sem drama, sem cobrança. Só marca a sessão como inativa.

---

## Quando leigo tá sumido há muito (> 90 dias)

IA ativa regra de `/vd-kill` preventivo:

> "Faz 90+ dias que você não mexe. Tudo bem. Algumas coisas podem ter
> mudado (IDE, plano de IA, suas próprias ideias). Quer:
> 1. Reler o que tinha e decidir se continua, mata, ou recomeça do zero
> 2. Atualizar a ideia (talvez não é mais o mesmo problema que te interessava)
> 3. Arquivar com `/vd-kill` (registra aprendizado e segue)"

Sem julgamento. Reconhece que ideia amadurece, contexto muda.
