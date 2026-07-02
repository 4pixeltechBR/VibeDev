# Handoff VibeDev ↔ Skills-Satélite

Este arquivo é a **referência canônica de contrato** entre a VibeDev (orquestrador de ciclo) e as skills-satélite (auditorias e decisões específicas). Toda skill-satélite aponta pra cá. Toda skill-satélite é esperada pra cumprir o que está aqui. VibeDev dispara skill-satélite pelo que está aqui.

**Princípio único**: o estado do projeto mora em `PROJECT_STATE.md`. Skills-satélite não criam estado próprio — elas **leem** o `PROJECT_STATE.md`, agem, e **escrevem o resultado de volta** no `PROJECT_STATE.md`, na seção da sub-tarefa ativa.

---

## 0. Modos de comunicação com o usuário

A skill-satélite pode operar em **dois modos** quando devolve saída ao usuário:

### Modo Técnico (padrão herdado)
Resposta direta em formato de risk register / dep verdict / postmortem — vocabulário fechado (C1-C8), gatilhos G#, verdicts OK/REVISAR/BLOQUEAR. Pensado pra engenheiro.

### Modo Leigo (camada conversacional)
Resposta renderizada pelos templates em `references/formato-conversa-leigo.md`, usando o dicionário de tradução em `references/glossario-leigo.md` e o painel visual em `references/estado-visual.md`. Pensado pra usuário sem formação em engenharia.

**Como decidir o modo**: o usuário declara (ex: "explica em linguagem simples" ou "fala técnico") OU a skill detecta pelo perfil do `PROJECT_STATE.md` (campo `modo_usuario` na raiz do estado). **Padrão é Modo Técnico** — pra ativar Modo Leigo por padrão, o `PROJECT_STATE.md` precisa ter `modo_usuario: leigo` explícito.

**Invariante**: independente do modo visível, o conteúdo escrito em `PROJECT_STATE.md` é **sempre técnico** (categorias C1-C8, gatilhos G#, verdicts canônicos). O modo leigo é só **renderização** — o registro interno não muda. Isso preserva auditoria.

### Documentos referenciados pelo Modo Leigo
- `references/glossario-leigo.md` — dicionário C1-C8 → linguagem natural
- `references/formato-conversa-leigo.md` — 3 templates de saída (decisão / aviso / panorama)
- `references/estado-visual.md` — painel resumido pro leigo ver o progresso

**Não é retrocompatível quebrar**: skill-satélite pode ignorar esses arquivos e rodar em Modo Técnico sem problema. Adicionar suporte ao Modo Leigo é **estritamente aditivo**.

---

## 1. Vocabulário comum (canônico)

Estes termos têm **significado único** nas duas pontas. Não renomeie, não traduzir, não abreviar.

| Termo | Significado |
|---|---|
| **Sub-tarefa ativa** | A única sub-tarefa registrada como ativa no `PROJECT_STATE.md`. Skills-satélite operam **somente** dentro dela. |
| **Decisão Tipo 1** | Decisão irreversível ou cara de reverter (banco, auth, stack, cobrança, exposição). Exige 3 opções + Red Team + sign-off. |
| **Decisão Tipo 2** | Decisão reversível e barata. Recomenda UMA opção em 1-2 linhas e segue. |
| **Gatilho G#** | Evento documentado neste handoff que dispara uma skill-satélite a partir da VibeDev. |
| **Saída estruturada** | Output da skill-satélite no formato fixo definido pelo handoff (risk register, dep verdict, postmortem draft, etc.). |
| **Critério de "pronto"** | Definido pela VibeDev no `/vd-plan` antes do código. Validado pela VibeDev no `/vd-check`. |
| **Modo DEBUG** | Quando `/vd-check` reprova. VibeDev muda estado pra `debug` na sub-tarefa e pode disparar G6. |
| **Estado do projeto** | `PROJECT_STATE.md` na raiz, fonte única de verdade. |

---

## 2. O ciclo básico (passo a passo)

```
[VibeDev] /vd-plan
    ↓ detecta keywords → aciona gatilho G# → dispara skill-satélite
[Skill-Satélite] lê PROJECT_STATE.md → age → escreve saída no PROJECT_STATE.md
    ↓
[VibeDev] recebe saída, monta plano com 3 opções (se Tipo 1) + critério de "pronto"
    ↓
[Usuário] aprova plano
    ↓
[VibeDev] /vd-build → pode disparar G# novamente se tocou gatilhos
    ↓
[VibeDev] /vd-check (validação por evidência)
    ↓ se reprovou → Modo DEBUG → G6 (skill-satélite modo audit)
    ↓ se aprovou → marca concluída + arquiva saída no Decision Log
```

**Regra de ouro**: skill-satélite **nunca inicia** sozinha. É sempre chamada por VibeDev durante um comando (`/vd-plan`, `/vd-build`, `/vd-check`) ou por G7 (chamada explícita do usuário).

**Regra de ouro 2**: skill-satélite **nunca decide** pelo usuário. Devolve opções com categorização; VibeDev transforma em decisão Tipo 1/Tipo 2; usuário aprova.

**Regra de ouro 3**: skill-satélite **nunca modifica** outras sub-tarefas no `PROJECT_STATE.md`. Apenas adiciona informação na sub-tarefa ativa.

---

## 3. Os 7 gatilhos documentados (G1-G7)

Estes são os **únicos** gatilhos que disparam skills-satélite. VibeDev não inventa gatilhos próprios; skills-satélite não inventam caminhos de entrada.

### G1 — Sub-tarefa envolve **autenticação**
**Palavras-gatilho**: `auth`, `login`, `senha`, `password`, `token`, `jwt`, `oauth`, `sessão`, `session`, `cookie`, `rbac`, `permission`, `role`, `permissionamento`, `mfa`, `2fa`, `magic link`, `clerk`, `auth0`, `nextauth`, `supabase.auth`, `firebase.auth`
**Categorias da skill-satélite envolvidas (padrão)**: C1 Autenticação & Sessão, C3 Segredos & Credenciais, C5 Criptografia & Dados Sensíveis
**Skill(s) esperada(s)**: `vibeshield`
**Momento**: pré-`/vd-build`

### G2 — Sub-tarefa envolve **persistência de dados**
**Palavras-gatilho**: `banco`, `database`, `db`, `migration`, `model`, `schema`, `seed`, `orm`, `sql`, `nosql`, `tabela`, `collection`, `persiste`, `armazenar`, `salvar`, `prisma`, `drizzle`, `sequelize`, `typeorm`, `sqlalchemy`, `mongoose`, `pg`, `mysql`, `postgres`, `mongodb`, `sqlite`, `redis`
**Categorias da skill-satélite envolvidas (padrão)**: C4 Validação & Sanitização, C5 Criptografia & Dados Sensíveis, C8 Logging & Observabilidade
**Skill(s) esperada(s)**: `vibeshield`
**Momento**: pré-`/vd-build`

### G3 — Sub-tarefa envolve **integração com API de terceiro**
**Palavras-gatilho**: `api`, `webhook`, `sdk`, `integração`, `third-party`, `stripe`, `github api`, `openai`, `anthropic`, `sendgrid`, `aws`, `gcp`, `azure`, `shopify`, `twilio`, `hubspot`, `slack`, `discord`, `google`, `meta`, `facebook`, `linkedin`
**Categorias da skill-satélite envolvidas (padrão)**: C3 Segredos & Credenciais, C7 Configuração & Deploy, C6 Dependências & Supply Chain
**Skill(s) esperada(s)**: `vibeshield`
**Momento**: pré-`/vd-build`

### G4 — Sub-tarefa envolve **exposição pública**
**Palavras-gatilho**: `deploy`, `produção`, `production`, `domínio`, `domain`, `cdn`, `host`, `vercel`, `netlify`, `railway`, `fly.io`, `aws`, `docker`, `k8s`, `kubernetes`, `ssl`, `tls`, `env var`, `variável de ambiente`, `cors`, `csp`, `helmet`, `rate limit`
**Categorias da skill-satélite envolvidas (padrão)**: C7 Configuração & Deploy, C3 Segredos & Credenciais
**Skill(s) esperada(s)**: `vibeshield`
**Momento**: pré-`/vd-build`

### G5 — **Dependência nova** entra no projeto
**Trigger técnico**: arquivo de dependência modificado (`package.json`, `package-lock.json`, `requirements.txt`, `Pipfile`, `pyproject.toml`, `Gemfile`, `Gemfile.lock`, `go.mod`, `Cargo.toml`, `composer.json`, `pubspec.yaml`) **e** dependência não estava em uso antes.
**Categorias**: C6 Dependências & Supply Chain
**Skill(s) esperada(s)**: `vibeshield` (via `/vibeshield-dep-check`)
**Momento**: durante validação `/vd-check` da sub-tarefa que adicionou a dep

### G6 — Sub-tarefa ativa já passou por **2 ciclos de `/vd-check` com reprovação**
**Trigger técnico**: linha no `PROJECT_STATE.md` da sub-tarefa ativa com `check_status: failed` aparecendo 2+ vezes para a mesma sub-tarefa.
**Categorias**: depende do contexto — skill-satélite re-roda em modo audit completo.
**Skill(s) esperada(s)**: `vibeshield` (modo debug-loop), ou futuras skills de debug.
**Momento**: quando VibeDev entra em Modo DEBUG.

### G7 — **Chamada explícita do usuário**
**Trigger**: usuário digita `/vibeshield-audit`, `/vibeshield-dep-check`, `/vibeshield-postmortem`, ou equivalente em linguagem natural ("audita isso", "faz varredura de segurança", "tô com medo desse deploy").
**Skill(s) esperada(s)**: qualquer uma das habilidades da skill-satélite.
**Momento**: a qualquer momento, mesmo fora de ciclo VibeDev.

---

## 4. Protocolo de disparo VibeDev → Skill-Satélite

Quando VibeDev detecta um gatilho G#, ela segue este protocolo **antes** de continuar o comando em curso:

```md
1. Identifica o gatilho (G#) lendo keywords na intenção da sub-tarefa.
2. Lê PROJECT_STATE.md para confirmar sub-tarefa ativa.
3. Escreve no PROJECT_STATE.md da sub-tarefa ativa:
   `trigger: G#[N] detected: <descrição do gatilho>`
4. Prepara o "pacote de contexto" pra skill-satélite:
   - ID da sub-tarefa ativa
   - Stack detectado (lê package.json / config / imports)
   - Trecho da intenção que disparou o gatilho
5. Chama a skill-satélite com: `/<skill> [comando] [id-sub-tarefa]`
   (skills-satélite reconhecem o id da sub-tarefa ativa no estado)
6. NÃO prossegue com o comando VibeDev até receber resposta da skill-satélite.
7. Recebe resposta, anexa no estado, continua o comando original.
```

**Invariante**: VibeDev **bloqueia** sua própria execução enquanto aguarda skill-satélite. Não é "rodar em paralelo e ver depois". É "pergunto, espero, integro, sigo".

---

## 5. Protocolo de execução da Skill-Satélite

Quando uma skill-satélite é chamada (via gatilho ou manualmente), ela segue este protocolo:

```md
1. Lê PROJECT_STATE.md. Identifica a sub-tarefa ativa.
2. Confere se o gatilho bate com a sub-tarefa ativa. Se não bate, devolve:
   `trigger_mismatch: sub-tarefa ativa não toca o gatilho. Abortando skill-satélite.`
   (evita disparar auditoria onde não deve)
3. Carrega contexto mínimo necessário: stack, intenção, histórico da sub-tarefa.
4. Executa a análise específica da skill (risk register, dep verdict, etc.).
5. Devolve saída no formato fixo definido em "6. Formato das saídas".
6. Escreve a saída no PROJECT_STATE.md, na seção da sub-tarefa ativa, com formato:
   `### [YYYY-MM-DD HH:MM] vs:[comando]
    [conteúdo da saída]`
7. NÃO modifica outras seções do estado.
8. NÃO executa ações no projeto (não roda comando, não edita código).
9. Devolve resposta curta pra VibeDev: `OK | REVISAR: [motivo] | BLOQUEAR: [motivo]`.
```

**Invariante**: skill-satélite **não tem mãos**. Ela analisa, categoriza, recomenda. Não toca no projeto.

---

## 6. Formato das saídas (canônico)

Toda saída de skill-satélite **deve** usar este envelope. O conteúdo interno varia, mas o envelope é fixo pra que a VibeDev possa parsear sem ambiguidade.

### Envelope externo (em `PROJECT_STATE.md`)

```md
### [YYYY-MM-DD HH:MM] [skill-id]:[comando] [verdict]
[corpo da saída]
---
**Próxima ação para VibeDev**: [uma das 3 abaixo]
1. `bloquear /vd-build até Tipo 1 com 3 opções` (quando verdict = BLOQUEAR)
2. `anexar este relatório ao Decision Log e seguir` (quando verdict = REVISAR)
3. `anexar como evidência do critério de "pronto"` (quando verdict = OK)
---
**Skill-satélite**: [vibeshield | futura]
**Trigger**: G#[N] | manual
**Categorias afetadas**: [C1] [C3] ...
```

### Saída do `/vibeshield-plan-check` (risk register)

```md
### [YYYY-MM-DD HH:MM] vibeshield:plan-check [verdict]

**Sub-tarefa**: [id/nome]
**Stack detectado**: [linguagem/framework/DB]
**Modo**: pré-build | pós-build | debug-loop | manual

#### 🔴 Bloqueante (bloqueia /vd-build)
- [ID-SEC-01] **C[N]** <descrição 1-2 frases>
  Padrão/CVE: [link ou "padrão conhecido: <ref>"]
  Recomendação: <1-2 frases>

#### 🟡 Requer decisão Tipo 1
- [ID-SEC-02] **C[N]** ...

#### 🟢 Aceitável na iteração atual, requer vigilância
- [ID-SEC-03] **C[N]** ...

#### ⚪ Não auditado
- [categoria]: [motivo]

---
**Próxima ação para VibeDev**: [ação padrão do envelope]
**Categorias afetadas**: [lista]
```

### Saída do `/vibeshield-dep-check`

```md
### [YYYY-MM-DD HH:MM] vibeshield:dep-check [verdict]

**Dependência**: [nome@versão ou arquivo]
**Categorias**: [C6]

#### 1. Mantenedor ativo?
[resposta + evidência]

#### 2. CVEs públicos?
[resposta + comando recomendado: ex: `npm audit --json | grep <pkg>`]

#### 3. Licença compatível?
[resposta]

#### 4. Pegada / bundle impact?
[estimativa]

#### 5. Risco de transitivas
[resposta]

**Verdict**: OK | REVISAR: [motivo] | BLOQUEAR: [motivo]

---
**Próxima ação para VibeDev**: [ação padrão]
```

### Saída do `/vibeshield-postmortem`

```md
### [YYYY-MM-DD HH:MM] vibeshield:postmortem [verdict]

**Data do incidente**: [ISO]
**Severidade estimada**: [baixa|média|alta|crítica]
**Categorização**: [STRIDE | OWASP Top 10 | NIST]
**Categorias afetadas**: [C1] [C3] ...

#### Linha do tempo
- HH:MM — <evento>
- HH:MM — <evento>

#### Causa raiz provisória
<2-4 frases>

#### Ação imediata sugerida (não executar)
- [bullet]

#### Ação de médio prazo
- [bullet]

#### Comunicação
- Quem precisa saber: [...]

---
**Próxima ação para VibeDev**: anexar como postmortem consolidado no /vd-close da sessão.
```

---

## 7. As 8 categorias canônicas de risco (vocabulário fechado)

Toda skill-satélite que devolve risk register **deve** classificar achados nestas 8 categorias. Categorias novas são proibidas; renomeação é proibida. Isso garante que risk register de 3 meses atrás é comparável com o atual.

| ID | Nome | Cobre | Exemplos típicos |
|---|---|---|---|
| **C1** | Autenticação & Sessão | Login, sessões, tokens, MFA | Falta de MFA em app com PII |
| **C2** | Autorização & Permissão | RBAC, IDOR, escalação | Endpoint que não checa owner |
| **C3** | Segredos & Credenciais | API keys, secrets em código, env vars | `STRIPE_SECRET` commitada |
| **C4** | Validação & Sanitização | SQLi, XSS, command injection, path traversal | Concatenação de string em SQL |
| **C5** | Criptografia & Dados Sensíveis | TLS, hashing de senha, PII em log | MD5 de senha |
| **C6** | Dependências & Supply Chain | CVEs em deps, pacotes abandonados, typosquatting | Dep sem release há 2 anos |
| **C7** | Configuração & Deploy | CORS, headers, debug em prod, secret em frontend | `Access-Control-Allow-Origin: *` em app autenticado |
| **C8** | Logging & Observabilidade | Audit log, PII em log, ausência de alertas | Sem log de tentativas falhas |

**Mapeamento OWASP Top 10 ↔ C#** (referência rápida, não é substituição):

| OWASP Top 10 (2021) | Categoria canônica |
|---|---|
| A01 Broken Access Control | C2 |
| A02 Cryptographic Failures | C5 |
| A03 Injection | C4 |
| A04 Insecure Design | (multi-C — geralmente C2 + C1) |
| A05 Security Misconfiguration | C7 |
| A06 Vulnerable & Outdated Components | C6 |
| A07 Identification & Auth Failures | C1 |
| A08 Software & Data Integrity Failures | C4 + C6 |
| A09 Security Logging & Monitoring Failures | C8 |
| A10 Server-Side Request Forgery | C4 |

---

## 8. Verdict canônico (status de retorno)

Toda saída de skill-satélite termina com um destes 3 verdicts. VibeDev lê esse verdict e age no comando atual.

| Verdict | Significado | Próxima ação da VibeDev |
|---|---|---|
| **`OK`** | Nenhum bloqueio. Pode prosseguir. | Anexar saída como evidência do critério de "pronto" no estado. |
| **`REVISAR: [motivo]`** | Riscos amarelos que não bloqueiam build mas exigem decisão Tipo 1 explícita. | Anexar saída ao Decision Log; mencionar nas 3 opções do `/vd-plan`. |
| **`BLOQUEAR: [motivo]`** | Pelo menos um achado vermelho. Não dá pra construir em cima. | Bloquear `/vd-build`; o usuário precisa fechar o ciclo com Tipo 1 + Red Team + sign-off antes de prosseguir. |

**Invariante**: um único achado vermelho = verdict `BLOQUEAR`. VibeDev não pode "aceitar o risco por conta própria" — sempre volta pro usuário.

---

## 9. Acesso ao estado (regras de leitura/escrita)

| Quem | Pode ler | Pode escrever | Em qual seção |
|---|---|---|---|
| VibeDev | Tudo | Tudo (no comando que está executando) | Todas |
| Skill-satélite | Tudo (precisa pra contexto) | Apenas adições na seção da sub-tarefa ativa | **Somente** "Sub-tarefa ativa" |
| Usuário | Tudo (controle total) | Tudo | Todas (via comandos) |

**Regra de só-acrescentar**: skill-satélite nunca **edita, remove ou reordena** conteúdo existente na seção da sub-tarefa. Apenas **adiciona** entradas timestamped no formato do envelope. Isso preserva auditoria: cada chamada da skill-satélite fica registrada.

**Regra de timestamp ISO**: timestamps usam ISO 8601 (`YYYY-MM-DD HH:MM`) para ordenação natural e legibilidade humana.

---

## 10. Situações de erro

### E1 — Skill-satélite chamada sem sub-tarefa ativa
**Comportamento**: devolve `error: no active sub-task. Execute /vd-status ou /vd-plan primeiro.`. VibeDev trata como `BLOQUEAR`.

### E2 — Skill-satélite chamada mas PROJECT_STATE.md não existe
**Comportamento**: devolve `error: PROJECT_STATE.md not found. VibeDev not initialized. Run /vd-start first.`. VibeDev trata como `BLOQUEAR` e instrui o usuário a rodar `/vd-start`.

### E3 — Gatilho disparado mas a skill correspondente não existe
**Comportamento**: VibeDev devolve `missing skill: [id]. Suppress with vd:skip:reason na sub-tarefa, ou instale a skill.`. O usuário escolhe se suprime (descartável) ou instala (segue ciclo).

### E4 — Skill-satélite devolve verdict mas estado corrompido
**Comportamento**: VibeDev lê o verdict da resposta direta da skill (não do estado) e pede confirmação ao usuário antes de prosseguir. Não assume coerência.

### E5 — Skill-satélite chamada em projeto Trilha Vermelha fase R1
**Comportamento**: a arqueologia tem prioridade. Skills-satélite podem ser chamadas só pra auditar o estado atual, **não** pra propor mudanças. O verdict padrão nesse caso é `REVISAR` em vez de `BLOQUEAR`, com nota explícita: *"Modo arqueologia: achado não bloqueia, mas deve entrar no Mapa do Caos."*

---

## 11. Estado final (template da sub-tarefa com skill-satélite)

Exemplo de como uma sub-tarefa fica no `PROJECT_STATE.md` depois de passar por skill-satélite:

```md
## Sub-tarefa ativa
**ID**: ST-007
**Nome**: Adicionar login com Google OAuth

### Contexto
[descrição resumida]

### Trigger
[G1 detectado em /vd-plan, 2026-06-30 01:15]
[G3 detectado em /vd-plan, 2026-06-30 01:15]

### Auditoria VibeShield
#### [2026-06-30 01:16] vibeshield:plan-check BLOQUEAR
[corpo da saída conforme /vibeshield-plan-check]

**Próxima ação para VibeDev**: bloquear /vd-build até Tipo 1 com 3 opções
**Categorias afetadas**: C1, C3

---

### Decisão (Tipo 1)
[3 opções com prós/contras + Red Team + escolha registrada]

### Build
[/vd-build executado]

### Check
[/vd-check executado]
```

A VibeDev pode ler essa seção e saber:
- Que gatilhos foram disparados.
- O que a skill-satélite devolveu (verdict + achados).
- Que decisão Tipo 1 foi tomada em resposta.
- O histórico completo de auditoria (não dá pra fraudar sem deixar rastro).

---

## 12. Regras de versionamento deste handoff

Qualquer skill-satélite nova que entre no ecossistema **deve** declarar conformidade com este handoff:

```md
# Skill: <nome>
**Conformidade com handoff-vibedev**: <versão> [sim/parcial/superseded]
**Adições ao envelope**: [nenhuma | <lista>]
**Categorias adicionadas**: [nenhuma | <C9+ — proibido sem PR no handoff>]
```

**Invariante**: nenhuma skill-satélite adiciona categoria nova sem atualizar este handoff e propagar pra VibeDev. Categorias novas exigem proposta + revisão de impacto + versionamento semver (`handoff-vibedev@1.1.0` → `1.2.0` se quebrar compatibilidade).

---

## 13. Anti-abuso

Padrões que **invalidam** o uso de skill-satélite mesmo que gatilhos batam:

1. **Trigger suppression**: usuário marca `vs:skip:reason` na sub-tarefa — válido só pra protótipo descartável (descritor: "fast prototype descartável, não vai pra produção"). Qualquer outro motivo é Bloqueio da VibeDev (decisão Tipo 1).
2. **Trigger flood**: chamar skill-satélite 3+ vezes na mesma sub-tarefa sem mudar contexto = pedir para a VibeDev parar a sub-tarefa e investigar causa (provavelmente sinal de drift).
3. **Manual override**: usuário diz "aceita o risco e segue" sem passar por Tipo 1 — VibeDev recusa, ponto. Exceção única: o usuário edita o `PROJECT_STATE.md` diretamente, registra o override no Decision Log, e segue. Auditoria manual é aceita; override silencioso, não.

---

**Este documento é canônico. Versão: 1.0.0. Última atualização: 2026-06-30.**
