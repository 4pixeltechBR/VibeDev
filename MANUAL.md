# Manual de Uso — VibeDev + VibeShield

## Índice

1. [Conceito geral](#conceito-geral)
2. [Iniciando um projeto](#iniciando-um-projeto)
3. [O ciclo VibeDev](#o-ciclo-vibedev)
4. [Quando a VibeShield entra](#quando-a-vibeshield-entra)
5. [Modo Técnico vs Modo Leigo](#modo-técnico-vs-modo-leigo)
6. [Exemplos práticos](#exemplos-práticos)
7. [Solução de problemas comuns](#solução-de-problemas-comuns)

## Conceito geral

Vibe Coding é programação onde você descreve o que quer em linguagem natural e a IA gera o código. Funciona rápido no começo, mas tem problemas conhecidos (dívida técnica, segurança frágil, debug em loop, etc.).

**VibeDev** resolve isso com governança de processo: define fases, força planejamento antes do código, registra decisões, valida por evidência.

**VibeShield** resolve a parte de segurança: quando você toca em autenticação, banco, integração ou deploy, a skill-satélite entra automaticamente e analisa antes de você seguir.

A coluna vertebral é `PROJECT_STATE.md` — fonte única de verdade. Toda skill lê e escreve nele.

## Iniciando um projeto

### Comando inicial
```
/vd-start
```

A IA vai perguntar (no máximo 3 perguntas):
1. Você já tem código escrito ou tá começando do zero?
2. Se tem código, está em produção?
3. O que o projeto faz?

Com base nas respostas, ela diagnostica a trilha (Verde = do zero, Vermelha = código existente com problemas) e cria o `PROJECT_STATE.md` com o template apropriado.

### Detecção automática de leigo

Se você responder com indícios de não-dev ("nunca programei", "tô começando", "não entendo de código"), a IA deve usar o template `PROJECT_STATE-green-leigo.md` e marcar `modo_usuario: leigo` no estado.

**Se isso não acontecer**, peça explicitamente:
```
Eu não sou dev. Por favor, releia o PROJECT_STATE.md e configure 
modo_usuario: leigo. Daqui pra frente, fala em linguagem simples.
```

## O ciclo VibeDev

### `/vd-plan [intenção em linguagem natural]`

Você fala o que quer, em linguagem natural. A IA devolve:
- Plano com sub-tarefas em ordem.
- Cada Tipo 1 (irreversível/caro) com 3 opções + prós/contras.
- Estimativa grosseira (🟢 15min / 🟡 1h / 🔴 2h+).
- Critério de "pronto" ANTES de qualquer código.

**Você aprova ou ajusta. Só segue pra `/vd-build` depois de aprovação explícita.**

### `/vd-build`

Executa o plano aprovado. **Uma sub-tarefa por vez**, com confirmação entre cada. Cada sub-tarefa crítica termina com pelo menos 1 teste smoke.

### `/vd-check`

Valida por evidência, não por afirmação. Você executa e descreve o resultado; a IA compara com o critério de "pronto" definido no `/vd-plan`.

### `/vd-close`

Encerra formalmente a sessão: atualiza estado, escreve micro-post-mortem (3 linhas: funcionou / revisaria / monitorar), sugere compactação se aplicável.

### `/vd-status`

Lê `PROJECT_STATE.md` e responde em 4 linhas: trilha, fase atual, última decisão, próximo passo. Use sempre que voltar de uma sessão.

## Quando a VibeShield entra

A VibeShield é satélite: ela só entra em pontos específicos do ciclo, automaticamente. Você não precisa chamá-la a cada passo.

### Gatilhos automáticos (G1-G6)

| Gatilho | Quando dispara | Categoria principal |
|---|---|---|
| G1 | Sub-tarefa toca **autenticação** | C1, C3 |
| G2 | Sub-tarefa toca **persistência de dados** | C4, C5, C8 |
| G3 | Sub-tarefa toca **integração com API de terceiro** | C3, C7 |
| G4 | Sub-tarefa toca **exposição pública / deploy** | C7, C3 |
| G5 | Dependência nova entra no projeto | C6 |
| G6 | Sub-tarefa reprovou 2x no `/vd-check` | (varia) |

### Gatilho manual (G7)

Você pode pedir auditoria a qualquer momento:
```
/vibeshield-audit [coisa]
```
ou em linguagem natural:
```
"Faz uma auditoria de segurança disso aqui"
```

### As 8 categorias (vocabulário fechado)

| ID | Tema |
|---|---|
| C1 | Autenticação & Sessão |
| C2 | Autorização & Permissão |
| C3 | Segredos & Credenciais |
| C4 | Validação & Sanitização |
| C5 | Criptografia & Dados Sensíveis |
| C6 | Dependências & Supply Chain |
| C7 | Configuração & Deploy |
| C8 | Logging & Observabilidade |

Em **Modo Técnico**, você vê esses IDs. Em **Modo Leigo**, eles viram perguntas em linguagem natural — você nunca vê "C3" como leigo.

### Os 3 verdicts

Toda análise da VibeShield termina com:

- **`OK`**: nada bloqueante. Segue.
- **`REVISAR: [motivo]`**: amarelos exigem decisão Tipo 1 explícita sua.
- **`BLOQUEAR: [motivo]`**: vermelho. Não dá pra construir em cima. Você precisa fechar a decisão antes de seguir.

## Modo Técnico vs Modo Leigo

### Modo Técnico (padrão, dev)

Saída em formato de risk register com IDs C1-C8, gatilhos G#, verdicts. Linguagem concisa, jargão técnico aceito. Você lê e decide.

### Modo Leigo (pessoa sem formação)

A skill detecta `modo_usuario: leigo` no `PROJECT_STATE.md` e renderiza:
- **Painel visual** com 5 passos, ícones de status, "você tá aqui".
- **3 opções com recomendação explícita** ("vai com a N porque...").
- **Sem jargão** — categoria C1 vira "Como as pessoas vão entrar no seu app?".
- **Fallbacks**: se você responder "não sei", ela muda o modo (analogia, caminho seguro default, ou pausa pra você pensar).
- **Glossário embutido**: termo técnico aparece com dica inline na primeira vez.
- **Regra dos 3 minutos**: se passar de 3 minutos de leitura, ela divide e pergunta por onde seguir.

### Trocar de modo

Pode trocar a qualquer momento. Modo Leigo é só renderização; o conteúdo no `PROJECT_STATE.md` continua técnico.

## Exemplos práticos

### Exemplo 1: Dev sênior fazendo SaaS

```
Você: /vd-plan adicionar login com Google no meu SaaS de notas

IA: Detectei gatilhos G1 e G3. VibeShield vai analisar antes do plano.

[VibeShield devolve risk register]
- 🔴 C3 — GOOGLE_CLIENT_SECRET precisa ir em .env ou vault
- 🔴 C7 — HTTPS obrigatório no callback OAuth
- 🟡 C5 — Token de sessão precisa de cuidado
- 🟡 C8 — Log de tentativas falhas

VibeDev: plano com 3 opções para C3 + 3 opções para C5. Você aprova?
```

### Exemplo 2: Leigo fazendo app de bolão

```
Você: Quero decidir como cada pessoa vai entrar no app. Pensei em 
Google porque é simples.

IA: 👋 Boa escolha. Vamos olhar com calma.

[Painel visual aparece com 5 passos da fase atual]

🔐 Onde você quer guardar a senha que conecta seu app ao Google?

3 opções:
1️⃣ Arquivo .env (recomendado pra você) — grátis, padrão da indústria
2️⃣ Serviço de Vault pago — bom pra times
3️⃣ Colar no código (não recomendo) — arriscado

🤖 Vai com a 1: é a primeira vez que você tá mexendo com login, 
então quanto mais simples melhor.

Qual você quer?
```

Exemplo completo em [vibeshield/examples/auth-google.md](./vibeshield/examples/auth-google.md).

## Solução de problemas comuns

### "A IA fala técnico demais"
Confirme `modo_usuario: leigo` em `PROJECT_STATE.md`. Se não estiver, peça pra IA adicionar. Se estiver e ela ainda assim falar técnico, é bug — reporte.

### "A VibeShield não ativa quando eu mexo em login"
Gatilhos dependem de keywords. Tente ser mais explícito: use palavras como "login", "banco de dados", "deploy", "API do Stripe". Se ainda não ativar, dispare manual: "antes de continuar, faz auditoria de segurança disso".

### "A IA aceitou uma decisão arriscada sem reclamar"
A VibeShield não substitui seu julgamento. Se você sentir que algo perigoso passou, peça: "Tem certeza? Olha isso de novo como se fosse um invasor tentando explorar."

### "O projeto tá com decisões malucas acumuladas"
Use `/vd-status` pra ver onde tá. Use `/vd-close` pra encerrar formalmente a sessão. Use `/vd-compact` se sugerido, pra arquivar decisões Tipo 2 de fases fechadas.

### "Em algum ponto travou e a IA não soube destravar"
Saia do ciclo e chame VibeShield manual: `/vibeshield-audit`. Se ela também travar, reveja a documentação em [vibeshield/references/handoff-vibedev.md](./vibeshield/references/handoff-vibedev.md).

## Roadmap

- **v1.1**: VibeResearch (skill-satélite de pesquisa pra decisões Tipo 1 com curadoria de evidência).
- **v1.2**: VibeDebug (skill-satélite de debug quando sub-tarefa reprova 3+ vezes em `/vd-check`).
- **v1.3**: VibeIndex (curadoria de bibliotecas e dependências).
- **v2.0**: Trilha Vermelha em Modo Leigo (resgate de projeto existente pra leigos).