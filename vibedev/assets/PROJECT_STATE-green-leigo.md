# PROJECT_STATE — [nome do projeto]

> Fonte única de verdade do VibeDev.
> A IA lê este arquivo no início de TODA sessão e atualiza ao final.
> Conversa contradiz arquivo → arquivo vence. Sinalize divergência.

**modo_usuario: leigo** ← esta versão do template é pra quem não é dev

---

## Contexto para novo agente/sessão
<!-- 3-5 linhas em linguagem natural. Última coisa que rolou + o que importa agora. -->




---

## Identidade
- **Projeto:**
- **Trilha:** 🟢 Verde (Greenfield)
- **Problema que resolve (1 frase do usuário que sofre):**
- **Para quem:**
- **Critério de sucesso:** o problema está resolvido quando...
- **Restrições:** [LGPD / orçamento / onde vai rodar]
- **Ambiente AI:** [Claude Code / Cursor / Antigravity / outro]
- **modo_usuario:** leigo
- **Criado em:** AAAA-MM-DD

---

## Mapa de fases (versão simples)
<!-- 5 fases em vez de 8. Linguagem leiga. Cada fase tem exemplos 
     do que se faz nela pra você não se perder. -->

- [ ] FASE 1 — Entender o problema
- [ ] FASE 2 — Decidir como construir
- [ ] FASE 3 — Construir com cuidado (inclui segurança)
- [ ] FASE 4 — Testar se funciona
- [ ] FASE 5 — Colocar no ar

Marcação: `[➔]` fase atual · `[x]` concluída · `[ ]` futura

---

## O que tá em cada fase (pra você não se perder)

| Fase | O que rola | Exemplo concreto |
|---|---|---|
| 1 — Entender o problema | Conversar sobre o que você quer, pra quem, e como vai saber que deu certo | "Quero um app onde meus amigos marcam presença no rolê" |
| 2 — Decidir como construir | Escolher ferramentas (linguagem, banco, hospedagem), com a ajuda do framework explicando prós e contras | "Vai usar Node.js porque tem mais gente que conhece" |
| 3 — Construir com cuidado | Escrever o código **com** decisões de segurança do começo. VibeShield entra aqui pra te ajudar. **Aqui também se discute custo mensal de manter isso rodando.** | "Login com Google funcionando, dados guardados com cuidado" |
| 4 — Testar | Conferir se funciona, se as travas tão ativas, se você entende o que tá pronto | "Posso logar, criar post, ver post, deslogar. Tudo certo?" |
| 5 — Colocar no ar | Publicar pro mundo, com HTTPS, monitorando erros | "Site no ar em meusite.com.br" |

---

## Custo mensal estimado (obrigatório fechar na Fase 3)

> Antes de escolher as ferramentas, você e a IA precisam fechar o custo
> estimado de manter o produto funcionando DEPOIS de pronto. Sem isso,
> o projeto pode crescer e o susto da conta chegar no fim do mês.
> Referência de valores: `references/estimativa-custos.md`.

| Porte de usuários | Custo estimado (BRL/mês) |
|---|---|
| 1-10 (validação inicial) | R$ |
| 10-100 (crescimento inicial) | R$ |
| 100-1000 (operação) | R$ |

- **Quem paga:** 
- **Decisão registrada em:** AAAA-MM-DD
- **Premissas:** 
- **Riscos de explosão de custo:** 

> ⚠️ Gate da Fase 3 → 4 **não passa** sem essa tabela preenchida.
> Se quiser pular, registre no log de decisões que pulou (decisão humana).

---

## Fase atual
- **Fase:** [número e nome]
- **Passo ativo:** [linguagem natural — ex: "decidir onde guardar a senha do banco"]
- **Tá pronto quando:** [o que você vai ver acontecer quando esse passo terminar]
- **Próximo passo:** [a próxima coisa concreta que vamos fazer]

---

## Decisões importantes (append-only — não apaga entrada antiga)

*Toda decisão de coisa importante (qual banco, como guardar senha, 
quem pode ver o quê) fica registrada aqui pra gente lembrar.*

| Quando | Decisão | Por que escolhemos |
|--------|---------|---------------------|
|        |         |                     |

---

## Tarefas pendentes (ideias que aparecem mas não viram código agora)

-

---

## Métricas (contadores do framework)
- retrabalho: 0 (teve que refazer algo)
- decisoes_revertidas: 0 (mudou de ideia depois)
- drift_detectado: 0 (saiu do plano)
- gates_reprovados: 0 (validação que falhou)

---

## Post-mortem de fase (quando uma fase termina)
<!-- 3 linhas por fase encerrada: funcionou / revisaria / monitorar -->
