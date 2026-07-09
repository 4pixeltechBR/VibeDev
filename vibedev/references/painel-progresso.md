# Painel Visual de Progresso — `/vd-status`

> Render alternativo do `/vd-status` quando `modo_usuario: leigo`.
> Substitui o formato 4-linhas técnico por uma representação visual
> mais intuitiva do estado do projeto.

---

## Quando usar

- `modo_usuario: leigo` no `PROJECT_STATE.md`
- Comando `/vd-status` (no Skeleton, no fechamento de sessão, ou chamado manual)

---

## Render canônico

**Fase 6 — Construção, sub-tarefa 3 de 8, 37% do projeto:**

```
═══════════════════════════════════════════
   MEU APP DE CONTAS DO CASAMENTO
═══════════════════════════════════════════

   Trilha: 🟢 Verde (começando do zero)
   Fase atual: 3 de 5 ─── ██████░░░░░░ 47%

   ✅ Fase 1 — Entender o problema
   ✅ Fase 2 — Decidir como construir
   ➔ Fase 3 — Construir com cuidado (VOCÊ ESTÁ AQUI)
   ⏸ Fase 4 — Testar
   ⏸ Fase 5 — Colocar no ar

   Nesta fase, você tá fazendo:
   ➔ Sub-tarefa 3 de 8: "tela de login com Pix"

   Última conquista (não tem nada ainda):
   • 04/07: criou conta no banco de dados (preparação)
   • 04/07: definiu como guardar senhas (Tipo 1)

   Custo estimado pra rodar (quando estiver no ar):
   • R$ 0-150/mês pros primeiros 10-100 usuários

   Próximo passo concreto:
   "Montar a tela de login com entrada de e-mail e senha."
═══════════════════════════════════════════
   Em caso de dúvida, fala: "/vd-status" de novo
═══════════════════════════════════════════
```

---

## Render quando Fase 1 (Validação)

```
═══════════════════════════════════════════
   MEU APP DE CONTAS DO CASAMENTO
═══════════════════════════════════════════

   Trilha: 🟢 Verde (começando do zero)
   Fase atual: 1 de 5 ─── █░░░░░░░░░░░ 12%

   ➔ Fase 1 — Entender o problema (VOCÊ ESTÁ AQUI)
   ⏸ Fase 2 — Decidir como construir
   ⏸ Fase 3 — Construir com cuidado
   ⏸ Fase 4 — Testar
   ⏸ Fase 5 — Colocar no ar

   Você tá definindo agora:
   • Problema real (1 frase do ponto de vista de quem sofre)
   • Pra quem é
   • O que é "deu certo"
   • Quando desistir

   Próxima coisa concreta:
   "Me conta o problema em 1 frase do ponto de vista
    de quem tá sofrendo com ele hoje, antes do seu app existir."

═══════════════════════════════════════════
```

---

## Render em modo técnico (preserva simplicidade)

```
Trilha: Verde
Fase 3 (Construção) — sub-tarefa 3/8
Última decisão: Padrão de hash de senha (bcrypt rounds=12)
Próximo: Tela de login com input e-mail/senha
```

Mantém as 4 linhas técnicas originais.

---

## Regras de render

1. **Largura fixa:** 39 caracteres úteis pra não quebrar em mobile.
2. **Barra de progresso:** cada bloco `█` = ~5% de progresso. 1 espaço = mesma unidade.
3. **Caracteres:** usar `█`, `░`, `✅`, `⏸`, `➔`. Evitar emoji adicional no painel.
4. **Unicode seguro:** garantir que apareça em terminal padrão. UTF-8.
5. **Pular campos vazios:** se não tem `custo_mensal_estimado` ainda, omite bloco inteiro.
6. **"Última conquista":** se não há marco registrado, fala "(nenhum marco registrado ainda)".

---

## Cores (recomendação, não obrigatório)

- Verde para concluído
- Amarelo para atual
- Cinza para futuro

ANSI: `\033[32m`, `\033[33m`, `\033[90m`. **Renderizar cores só se
terminal suportar**. Default sem cor (leigos em terminal plain ganham
a mesma info sem cor).

---

## Como preencher campos

| Campo | Fonte |
|---|---|
| Nome | `Identidade → Projeto` no `PROJECT_STATE.md` |
| Trilha | `Identidade → Trilha` |
| Fase atual % | `(fase_atual_numero / total_fases) * 100` |
| Conquistas | `Post-mortem de fase` ou `decision_log` filtrado |
| Custo | `custo_mensal_estimado` |
| Próximo passo | `Fase atual → Passo ativo` |

---

## Integração com gate de fase

Ao passar de fase, render mostra "🎉 fase fechada" apenas se for fase
significativa (Fases 1, 3, 5). Outras transições são invisíveis.

---

## Limites

Painel visual **não substitui** o conteúdo do estado. É só uma camada
de leitura. Decisões e logs continuam no `PROJECT_STATE.md`.

Em projetos com `modo_usuario: tecnico`, painel visual **não roda**.
Status técnico usa 4 linhas simples.
