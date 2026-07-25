# /vd-help

> Router do VibeDev. Use quando você tá perdido, não sabe qual comando chamar,
> ou o framework te deixou confuso.

## Tipo de invocação

**User-invoked.** Só humano chama. Não dispara automaticamente.

Inspirado por `ask-matt` em [mattpocock/skills](https://github.com/mattpocock/skills)
(referência competitiva, ver `CREDITS.md`), mas adaptado pra leigo.

## O que é

Você tem 12 comandos. Não precisa decorar todos. O `/vd-help` olha onde
você tá no projeto e sugere **o próximo passo concreto**.

**Nota:** o 12º comando é `/vd-arch-review` — auditoria arquitetural pra
devs. Leigos podem ignorar.

## Mapa mental — o ciclo normal

```
1. Tô começando do zero?
   → /vd-spark (extrai a ideia em 4 rodadas)
   → /vd-start (cria o projeto)
   → /vd-status (vê onde tá)

2. Tô no meio do projeto
   → /vd-status (orienta)
   → /vd-plan (planeja próxima coisa)
   → /vd-build (executa)
   → /vd-check (valida se funcionou)

3. Tô em dúvida se o que fiz tá bom
   → /vd-check (valida explicitamente)

4. Tô travado há dias / sem energia
   → /vd-kill (encerra com dignidade, salva aprendizado)

5. Tô lançando (gate Fase 7→8)
   → /vd-launch (blocos de comunicação + checklist)

6. Tô encerrando a sessão de hoje
   → /vd-close (atualiza estado + post-mortem)
```

## Mapa mental — pelas fases

| Fase | Comando principal | Quando sai |
|---|---|---|
| 1 — Validação | `/vd-spark` → `/vd-start` → `/vd-status` | Quando tem 1 frase clara do problema |
| 2 — Especificação | `/vd-plan` | Quando tem lista de features + anti-escopo |
| 3 — Arquitetura | `/vd-plan` (com tabela de custo) | Quando tem stack + custo estimado |
| 4 — Segurança | (embutida em 6) | Quando auth/dados estão prontos |
| 5 — Versão | (embutida em 6) | Quando tem git + estrutura |
| 6 — Construção | `/vd-build` + `/vd-check` | Quando sub-tarefas passam no /vd-check |
| 7 — Homologação | `/vd-check` (rodar pra cada feature) | Quando usuário real completa o fluxo |
| 8 — Operação | `/vd-launch` → monitoramento | Quando tá no ar e estável |

## Comandos por situação emocional

| Você tá... | Comando | Por quê |
|---|---|---|
| animado e com energia | `/vd-build` | Executa o plano |
| exausto e sem saco | `/vd-kill` ou `/vd-close` | Reconhece o cansaço, salva o que tem |
| confuso e sem saber | `/vd-help` (você tá aqui) | Volta a clareza |
| ansioso pra lançar | `/vd-status` | Vê se gate Fase 7→8 já passou |
| travado numa decisão técnica | `/vd-plan` (Tipo 1) | 3 opções + Red Team |
| frustrado porque "tá tudo errado" | `/vd-check` | Valida o que realmente tá pronto |

## Quando chamar (auto-regras)

O framework **anuncia** se você está em uma dessas situações:
- 7+ dias sem mexer → sugere recap (você pode aceitar ou pular)
- Detecção de ideia nova fora do escopo → anota no backlog (você pode editar)
- Modo País detectado → ativa contexto (você pode desativar)
- Glossário ativo em modo leigo → traduz jargão (você pode pedir pra parar)

Quando esses anúncios rolarem, **leia**. Não assuma que é só ruído.

## O que o /vd-help NÃO faz

- **Não decide por você.** Ele sugere, você escolhe.
- **Não pula comando.** Se a fase pede `/vd-plan`, não dá pra ir direto pra `/vd-build`.
- **Não substitui o framework inteiro.** É só o mapa dos comandos.

## Comando de emergência

Se nada disso fizer sentido, e você só quer **sair do framework com dignidade**:

```
/vd-kill
```

Encerra o projeto, salva o aprendizado no `IDEA_LOG.md`, e te deixa respirar.

Sair é tão válido quanto terminar.

---

## Fluxo Principal VibeDev (versão resumida)

Se você só quer saber **a ordem geral** sem se perder:

```
1. Tem ideia?              /vd-spark
2. Começar projeto?        /vd-start
3. Onde estou?             /vd-status
4. Próxima coisa?          /vd-plan
5. Fazer?                  /vd-build
6. Funcionou de verdade?   /vd-check
7. Lançar?                 /vd-launch
8. Encerrar sessão?        /vd-close
9. Travou?                 /vd-kill
```

**Dica:** se você só lembrar de UM comando, lembre do `/vd-status`. Ele te diz onde tá.
Se não souber qual usar, `/vd-help` (esse aqui).
