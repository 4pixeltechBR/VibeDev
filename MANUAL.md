# Manual de Uso / User Manual

> 🇺🇸 English follows; 🇧🇷 Português abaixo.

---

## 🇺🇸 English

### Table of Contents

1. [General concept](#general-concept)
2. [Starting a project](#starting-a-project)
3. [The VibeDev cycle](#the-vibedev-cycle)
4. [When VibeShield activates](#when-vibeshield-activates)
5. [Technical vs Layman Mode](#technical-vs-layman-mode)
6. [Practical examples](#practical-examples)
7. [Common troubleshooting](#common-troubleshooting)

### General concept

Vibe Coding is programming where you describe what you want in natural language and the AI generates the code. It works fast at the beginning, but has known problems (technical debt, fragile security, debug loops, etc.).

**VibeDev** solves this with process governance: defines phases, forces planning before code, records decisions, validates by evidence.

**VibeShield** solves the security part: when you touch authentication, database, integration, or deploy, the satellite skill enters automatically and analyzes before you proceed.

The backbone is `PROJECT_STATE.md` — single source of truth. Every skill reads and writes to it.

### Starting a project

#### Initial command

```
/vd-start
```

The AI will ask (maximum 3 questions):
1. Do you already have written code or are you starting from scratch?
2. If you have code, is it in production?
3. What does the project do?

Based on the answers, it diagnoses the track (Green = from scratch, Red = existing code with problems) and creates `PROJECT_STATE.md` with the appropriate template.

#### Automatic layman detection

If you respond with signs of non-dev ("never programmed", "I'm starting", "I don't understand code"), the AI should use the `PROJECT_STATE-green-leigo.md` template and mark `modo_usuario: leigo` in the state.

**If this doesn't happen**, ask explicitly:
```
I'm not a dev. Please, re-read PROJECT_STATE.md and configure 
modo_usuario: leigo. From now on, speak in simple language.
```

### The VibeDev cycle

#### `/vd-plan [natural language intention]`

You speak what you want, in natural language. The AI returns:
- Plan with sub-tasks in order.
- Each Type 1 (irreversible/expensive) with 3 options + pros/cons.
- Rough estimate (🟢 15min / 🟡 1h / 🔴 2h+).
- "Done" criterion BEFORE any code.

**You approve or adjust. Only proceed to `/vd-build` after explicit approval.**

#### `/vd-build`

Executes the approved plan. **One sub-task at a time**, with confirmation between each. Each critical sub-task ends with at least 1 smoke test.

#### `/vd-check`

Validates by evidence, not by assertion. You execute and describe the result; the AI compares with the "done" criterion defined in `/vd-plan`.

#### `/vd-close`

Formally ends the session: updates state, writes micro-post-mortem (3 lines: worked / would revise / monitor), suggests compaction if applicable.

#### `/vd-status`

Reads `PROJECT_STATE.md` and responds in 4 lines: track, current phase, last decision, next step. Use whenever returning from a session.

### When VibeShield activates

VibeShield is a satellite: it only enters at specific points of the cycle, automatically. You don't need to call it at every step.

#### Automatic triggers (G1-G6)

| Trigger | When it fires | Main category |
|---|---|---|
| G1 | Sub-task touches **authentication** | C1, C3 |
| G2 | Sub-task touches **data persistence** | C4, C5, C8 |
| G3 | Sub-task touches **third-party API integration** | C3, C7 |
| G4 | Sub-task touches **public exposure / deploy** | C7, C3 |
| G5 | New dependency enters the project | C6 |
| G6 | Sub-task failed 2x in `/vd-check` | (varies) |

#### Manual trigger (G7)

You can request an audit at any time:
```
/vibeshield-audit [thing]
```
or in natural language:
```
"Do a security audit on this"
```

#### The 8 categories (closed vocabulary)

| ID | Theme |
|---|---|
| C1 | Authentication & Session |
| C2 | Authorization & Permission |
| C3 | Secrets & Credentials |
| C4 | Validation & Sanitization |
| C5 | Encryption & Sensitive Data |
| C6 | Dependencies & Supply Chain |
| C7 | Configuration & Deploy |
| C8 | Logging & Observability |

In **Technical Mode**, you see these IDs. In **Layman Mode**, they become natural language questions — you never see "C3" as a layman.

#### The 3 verdicts

Every VibeShield analysis ends with:

- **`OK`**: nothing blocking. Proceed.
- **`REVIEW: [reason]`**: yellows require explicit Type 1 decision from you.
- **`BLOCK: [reason]`**: red. Cannot build on top. You need to close the decision before proceeding.

### Technical vs Layman Mode

#### Technical Mode (default, dev)

Output in risk register format with C1-C8 IDs, G# triggers, verdicts. Concise language, technical jargon accepted. You read and decide.

#### Layman Mode (non-technical person)

The skill detects `modo_usuario: leigo` in `PROJECT_STATE.md` and renders:
- **Visual panel** with 5 steps, status icons, "you are here".
- **3 options with explicit recommendation** ("go with N because...").
- **No jargon** — C1 category becomes "How will people log into your app?".
- **Fallbacks**: if you respond "I don't know", it changes mode (analogy, safe default path, or pause for you to think).
- **Inline glossary**: technical term appears with inline tip on first use.
- **3-minute rule**: if it exceeds 3 minutes of reading, it divides and asks where to continue.

### Practical examples

See [vibeshield/examples/auth-google.md](./vibeshield/examples/auth-google.md) for a complete walkthrough of adding Google OAuth login.

### Common troubleshooting

#### "The AI speaks too technical"
Confirm `modo_usuario: leigo` in `PROJECT_STATE.md`. If absent, ask AI to add it.

#### "VibeShield does not activate when I touch login"
Triggers depend on keywords. Try being more explicit: use words like "login", "database", "deploy", "Stripe API". If still doesn't activate, fire manual: "before continuing, do a security audit on this".

#### "The AI accepted a risky decision without complaining"
VibeShield does not replace your judgment. If you feel something dangerous passed, ask: "Are you sure? Look at this again as if you were an attacker trying to exploit it."

---

## 🇧🇷 Português

### Índice

1. [Conceito geral](#conceito-geral-pt)
2. [Iniciando um projeto](#iniciando-um-projeto-pt)
3. [O ciclo VibeDev](#o-ciclo-vibedev-pt)
4. [Quando a VibeShield entra](#quando-a-vibeshield-entra-pt)
5. [Modo Técnico vs Modo Leigo](#modo-técnico-vs-modo-leigo-pt)
6. [Exemplos práticos](#exemplos-práticos-pt)
7. [Solução de problemas comuns](#solução-de-problemas-comuns-pt)

### Conceito geral (PT)

Vibe Coding é programação onde você descreve o que quer em linguagem natural e a IA gera o código. Funciona rápido no começo, mas tem problemas conhecidos (dívida técnica, segurança frágil, debug em loop, etc.).

**VibeDev** resolve isso com governança de processo: define fases, força planejamento antes do código, registra decisões, valida por evidência.

**VibeShield** resolve a parte de segurança: quando você toca em autenticação, banco, integração ou deploy, a skill-satélite entra automaticamente e analisa antes de você seguir.

A coluna vertebral é `PROJECT_STATE.md` — fonte única de verdade. Toda skill lê e escreve nele.

### Iniciando um projeto (PT)

#### Comando inicial

```
/vd-start
```

A IA vai perguntar (no máximo 3 perguntas):
1. Você já tem código escrito ou tá começando do zero?
2. Se tem código, está em produção?
3. O que o projeto faz?

Com base nas respostas, ela diagnostica a trilha (Verde = do zero, Vermelha = código existente com problemas) e cria o `PROJECT_STATE.md` com o template apropriado.

#### Detecção automática de leigo

Se você responder com indícios de não-dev ("nunca programei", "tô começando", "não entendo de código"), a IA deve usar o template `PROJECT_STATE-green-leigo.md` e marcar `modo_usuario: leigo` no estado.

**Se isso não acontecer**, peça explicitamente:
```
Eu não sou dev. Por favor, releia o PROJECT_STATE.md e configure 
modo_usuario: leigo. Daqui pra frente, fala em linguagem simples.
```

### O ciclo VibeDev (PT)

#### `/vd-plan [intenção em linguagem natural]`

Você fala o que quer, em linguagem natural. A IA devolve:
- Plano com sub-tarefas em ordem.
- Cada Tipo 1 (irreversível/caro) com 3 opções + prós/contras.
- Estimativa grosseira (🟢 15min / 🟡 1h / 🔴 2h+).
- Critério de "pronto" ANTES de qualquer código.

**Você aprova ou ajusta. Só segue pra `/vd-build` depois de aprovação explícita.**

#### `/vd-build`

Executa o plano aprovado. **Uma sub-tarefa por vez**, com confirmação entre cada. Cada sub-tarefa crítica termina com pelo menos 1 teste smoke.

#### `/vd-check`

Valida por evidência, não por afirmação. Você executa e descreve o resultado; a IA compara com o critério de "pronto" definido no `/vd-plan`.

#### `/vd-close`

Encerra formalmente a sessão: atualiza estado, escreve micro-post-mortem (3 linhas: funcionou / revisaria / monitorar), sugere compactação se aplicável.

#### `/vd-status`

Lê `PROJECT_STATE.md` e responde em 4 linhas: trilha, fase atual, última decisão, próximo passo. Use sempre que voltar de uma sessão.

### Quando a VibeShield entra (PT)

A VibeShield é satélite: ela só entra em pontos específicos do ciclo, automaticamente. Você não precisa chamá-la a cada passo.

#### Gatilhos automáticos (G1-G6)

| Gatilho | Quando dispara | Categoria principal |
|---|---|---|
| G1 | Sub-tarefa toca **autenticação** | C1, C3 |
| G2 | Sub-tarefa toca **persistência de dados** | C4, C5, C8 |
| G3 | Sub-tarefa toca **integração com API de terceiro** | C3, C7 |
| G4 | Sub-tarefa toca **exposição pública / deploy** | C7, C3 |
| G5 | Dependência nova entra no projeto | C6 |
| G6 | Sub-tarefa reprovou 2x no `/vd-check` | (varia) |

#### Gatilho manual (G7)

Você pode pedir auditoria a qualquer momento:
```
/vibeshield-audit [coisa]
```
ou em linguagem natural:
```
"Faz uma auditoria de segurança disso aqui"
```

#### As 8 categorias (vocabulário fechado)

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

#### Os 3 verdicts

Toda análise da VibeShield termina com:

- **`OK`**: nada bloqueante. Segue.
- **`REVISAR: [motivo]`**: amarelos exigem decisão Tipo 1 explícita sua.
- **`BLOQUEAR: [motivo]`**: vermelho. Não dá pra construir em cima. Você precisa fechar a decisão antes de seguir.

### Modo Técnico vs Modo Leigo (PT)

#### Modo Técnico (padrão, dev)

Saída em formato de risk register com IDs C1-C8, gatilhos G#, verdicts. Linguagem concisa, jargão técnico aceito. Você lê e decide.

#### Modo Leigo (pessoa sem formação)

A skill detecta `modo_usuario: leigo` no `PROJECT_STATE.md` e renderiza:
- **Painel visual** com 5 passos, ícones de status, "você tá aqui".
- **3 opções com recomendação explícita** ("vai com a N porque...").
- **Sem jargão** — categoria C1 vira "Como as pessoas vão entrar no seu app?".
- **Fallbacks**: se você responder "não sei", ela muda o modo (analogia, caminho seguro default, ou pausa pra você pensar).
- **Glossário embutido**: termo técnico aparece com dica inline na primeira vez.
- **Regra dos 3 minutos**: se passar de 3 minutos de leitura, ela divide e pergunta por onde seguir.

### Exemplos práticos (PT)

Veja [vibeshield/examples/auth-google.md](./vibeshield/examples/auth-google.md) para um walkthrough completo de adicionar login com Google OAuth.

### Solução de problemas comuns (PT)

#### "A IA fala técnico demais"
Confirme `modo_usuario: leigo` em `PROJECT_STATE.md`. Se não estiver, peça pra IA adicionar. Se estiver e ela ainda assim falar técnico, é bug — reporte.

#### "A VibeShield não ativa quando eu mexo em login"
Gatilhos dependem de keywords. Tente ser mais explícito: use palavras como "login", "banco de dados", "deploy", "API do Stripe". Se ainda não ativar, dispare manual: "antes de continuar, faz auditoria de segurança disso".

#### "A IA aceitou uma decisão arriscada sem reclamar"
A VibeShield não substitui seu julgamento. Se você sentir que algo perigoso passou, peça: "Tem certeza? Olha isso de novo como se fosse um invasor tentando explorar".