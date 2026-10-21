# /vd-help

A pessoa chamou porque perdeu o fio. Responda com o próximo comando e pare. Não recite o framework.

`SKILL.md` vence se este arquivo divergir.

## Próximo passo

| Situação | Comando |
|---|---|
| Só uma ideia, ainda sem `PROJECT_STATE.md` | Fatia (as 4 perguntas numa mensagem, depois a menor tela) |
| Já tem código e está quebrado | Resgate (3 perguntas, conserta só o desta semana) |
| Quer saber onde está | `/vd-status` |
| Quer decidir o próximo pedaço | `/vd-plan` |
| O plano já foi aceito | `/vd-build` |
| Quer saber se funcionou | `/vd-check` |
| Vai embora hoje | `/vd-close` |
| Quer parar o projeto | `/vd-kill` |
| Mapa leigo | 1 Entender o problema · 2 Decidir como construir · 3 Construir · 4 Testar · 5 Colocar no ar |
| Mapa técnico de 8 fases | Só se `modo_usuario: tecnico`. Abra `references/trilha-verde.md` |

`/vd-arch-review` é auditoria para quem já programa. Não ofereça no modo leigo.

## Uma linha por comando

```
Fatia        ideia → uma coisa visível
/vd-status   onde estou
/vd-plan     próximo pedaço, e para
/vd-build    faz o plano inteiro
/vd-check    aconteceu na tela agora?
/vd-launch   depois que Testar passou
/vd-close    salva e encerra a sessão
/vd-kill     encerra o projeto e guarda o aprendizado
```

Se ela só for lembrar de um, é `/vd-status`.
