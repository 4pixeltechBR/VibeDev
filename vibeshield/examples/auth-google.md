# Exemplo Completo — Login com Google OAuth

> Este exemplo simula **uma sessão real** do ciclo VibeDev + VibeShield pra adicionar login com Google num projeto greenfield de "App de Lista de Tarefas". Mostra como as duas skills conversam em **Modo Técnico** e em **Modo Leigo** paralelamente.
>
> Use este exemplo pra: (a) entender o fluxo de ponta a ponta, (b) testar se o framework atende o seu caso, (c) treinar alguém a usar a VibeShield.

---

## Setup inicial

`PROJECT_STATE.md` na raiz do projeto (resumo):

```md
## Identidade
- **Projeto:** tasky-app
- **Trilha:** 🟢 Verde
- **Problema:** "Quero um app simples pra listar minhas tarefas do dia"
- **Para quem:** Eu mesmo (uso pessoal), eventualmente amigos
- **Critério de sucesso:** Consigo adicionar tarefa, marcar como feita, ver histórico
- **Ambiente AI:** Claude Code
- **modo_usuario: leigo**

## Fase atual
- **Fase:** 4 — Segurança & Dados
- **Sub-tarefa ativa:** 4.2 — Adicionar login com Google OAuth
- **Está pronto quando:** Consigo logar com conta Google, sessão persiste entre acessos, logout limpa a sessão
- **Próximo passo:** Decidir entre implementar OAuth direto ou usar serviço gerenciado
```

A intenção do usuário: "Quero que as pessoas usem a conta do Google pra entrar no app".

---

## Fase 1 — VibeDev detecta gatilhos

Quando o usuário pede pra planejar "Adicionar login com Google", VibeDev lê `PROJECT_STATE.md`, identifica a sub-tarefa 4.2, e **escaneia palavras-gatilho**:

| Palavra encontrada | Gatilho |
|---|---|
| `login` | G1 (autenticação) |
| `Google` | G3 (integração API terceiro — Google OAuth é via API) |

Dois gatilhos batem. VibeDev ativa VibeShield **antes** de gerar o plano.

---

## Fase 2 — VibeShield roda a análise (interno)

VibeShield lê `PROJECT_STATE.md`, identifica stack (a definir pelo package.json), e roda mentalmente pelas 8 categorias pra sub-tarefa "Login com Google OAuth":

| Cat | Análise interna | Achado |
|---|---|---|
| **C1** Auth | Vai faltar mecanismo de logout e sessão persistente? | 🟡 provável |
| **C2** Permissão | Cada usuário vê só as próprias tarefas (regra do projeto)? | 🟡 provável |
| **C3** Segredos | Vai precisar do `GOOGLE_CLIENT_SECRET` em algum lugar | 🔴 certo |
| **C4** Validação | | ⚪ não se aplica |
| **C5** Dados sensíveis | Token do Google pode ser persistido? | 🟡 provável |
| **C6** Dependências | Vai entrar dep nova (lib do Google)? | 🟡 provável |
| **C7** Config | HTTPS obrigatório no callback | 🔴 certo |
| **C8** Logs | Log de login attempts? | 🟢 provavelmente não |

Como tem 🔴 em C3 e C7, verdict interno é `BLOQUEAR`. Categorias que importam pra mostrar pro usuário: **C1, C3, C7**.

---

## Fase 3 — VibeShield detecta modo

VibeShield lê `modo_usuario: leigo` no `PROJECT_STATE.md` → **Modo Leigo ativado**.

Carrega os 3 references do Modo Leigo:
- `glossario-leigo.md` (pra C1, C3, C7)
- `formato-conversa-leigo.md` (template Decisão Necessária)
- `estado-visual.md` (painel inicial)

---

## Fase 4 — Resposta do VibeShield (Modo Leigo)

Aqui está o que o usuário **leigo** veria na conversa. A VibeShield renderiza o painel primeiro, depois as decisões.

### 4.1 — Painel inicial

```md
┌────────────────────────────────────────────────┐
│ 🛡️ VibeShield — Vamos olhar seu projeto       │
└────────────────────────────────────────────────┘

👋 Olá! Eu tava te esperando nessa parte — 
login é uma das decisões mais importantes do 
app. Vamos com calma.

Tô vendo que a gente tá na Fase 4 (Segurança 
& Dados) e você quer adicionar o "Entrar com 
Google". Boa escolha — é o caminho mais simples.

A gente tem 3 decisões pra tomar hoje, nessa 
ordem:

  ┌────────────────────────────────────┐
  │ 📍 Você tá aqui                    │
  │                                    │
  │ 🔴 1. Decidir como guardar a chave │
  │      do Google com segurança       │
  │                                    │
  │ ⏸️  2. Decidir se você quer MFA     │
  │      (verificação em duas etapas) │
  │                                    │
  │ ⏸️  3. Garantir que o site tá       │
  │      usando HTTPS (cadeado)        │
  └────────────────────────────────────┘

❓ Topa começar pelo passo 1?
```

### 4.2 — Decisão 1: guardar o `GOOGLE_CLIENT_SECRET`

```md
🔐 **Onde você quer guardar a senha que conecta seu app ao Google?**

Quando você configura o "Entrar com Google", o Google te dá duas coisas: 
um identificador público e uma senha (chamada "client secret"). Essa 
senha é tipo a chave da porta dos fundos — quem tiver ela pode se passar 
pelo seu app.

A gente **não pode** colar essa senha direto no código. Senão, qualquer 
pessoa que abrir seu projeto no GitHub vê ela.

Eu te mostro 3 jeitos de guardar isso:

---

### As 3 opções

**1️⃣ Arquivo `.env` (recomendado pra você)**
Você cria um arquivo chamado `.env` na raiz do seu projeto. Nele você 
coloca a senha do Google. O seu código lê a senha desse arquivo. Tem 
um arquivo `.gitignore` que garante que esse `.env` **nunca vai pro 
GitHub**. É grátis, padrão da indústria, todo dev conhece.

📌 *Quando faz sentido:* Quase sempre. Funciona pra projetos pequenos 
e médios.

💸 *Custo/complexidade:* Grátis. Leva 5 minutos pra configurar.

---

**2️⃣ Serviço de "Vault" pago (Doppler, Infisical, etc.)**
Esses serviços guardam suas senhas em nuvem, com criptografia extra, e 
deixam você trocar a senha sem mexer no código. Bom pra equipes.

📌 *Quando faz sentido:* Quando você tá trabalhando com mais gente, ou 
quando o app é sensível (saúde, banco, documento).

💸 *Custo/complexidade:* Tem versão grátis básica, paga a partir de ~$10/mês.

---

**3️⃣ Colar a senha direto no código (não recomendo)**
Funciona, mas é perigoso. A senha vai vazar se você um dia subir o 
código pro GitHub, e qualquer um que acessar seu repositório vai ver.

📌 *Quando faz sentido:* Só pra teste **bem rápido** em computador 
pessoal, descartável.

💸 *Custo/complexidade:* Zero, mas arriscado.

---

### 🤖 Minha recomendação

**Vai com a 1 (`arquivo .env`)** porque é a primeira vez que você tá 
adicionando login, então quanto mais simples melhor. Esse é o padrão 
que 90% dos devs usam. Se um dia crescer e você precisar compartilhar 
senhas com um time, migra pra opção 2.

### ⚠️ O que acontece se você escolher outra

Se escolher a 3 (colar no código), você pode esquecer de tirar a 
senha antes de subir pro GitHub. Aí qualquer pessoa com o link do 
repositório vai poder logar no Google fingindo ser seu app. É o tipo 
de vazamento que destrói empresa.

---

**Decisão tua.** Qual você quer?
```

### 4.3 — Usuário responde "1, opção 1"

VibeShield registra a decisão no estado (Modo Técnico, internamente) e segue pro passo 2.

### 4.4 — Decisão 2 (MFA) — Achado amarelo, decisão Tipo 2

```md
🔐 **Verificação em duas etapas (MFA)?**

Como o app é só pra uso pessoal (e eventualmente amigos), você não 
precisa adicionar verificação em duas etapas agora. Mas me conta: 
você quer que eu recomende? Porque se um dia você esquecer a senha 
do Google e a conta tiver 2FA ligada, recuperar é mais chato (mas 
mais seguro).

**A maioria dos apps pessoais começa sem MFA.** Se quiser, a gente 
deixa uma anotação pra voltar nisso antes de publicar publicamente.

Topa deixar pra depois, ou quer configurar agora?
```

### 4.5 — Decisão 3 (HTTPS) — Achado vermelho

```md
🔐 **Cadeado no navegador (HTTPS)?**

Esse aqui é importante: quando você botar seu app na internet, o 
site PRECISA ter aquele cadeado. Sem ele, a senha que a pessoa 
digita pra entrar viaja pela internet legível — qualquer um 
"escutando" a rede vê.

A boa notícia: hospedagem moderna tipo Vercel, Netlify, Railway, 
Fly.io já configuram HTTPS pra você automaticamente. Então não é 
mais uma decisão que você precisa tomar — só precisa **lembrar** 
de não hospedar num lugar que não tenha HTTPS.

✅ *Minha recomendação:* sem ação agora. A gente confere de novo 
quando você for publicar.
```

---

## Fase 5 — Estado atualizado (Modo Técnico, interno)

Aqui é o que **o framework escreve em `PROJECT_STATE.md`** durante a sessão. O leigo não vê isso; o dev que audita, sim.

```md
### Sub-tarefa ativa: 4.2 — Adicionar login com Google OAuth

#### Trigger
[G1 detectado em /vd-plan, 2026-06-30 14:22]
[G3 detectado em /vd-plan, 2026-06-30 14:22]

#### Auditoria VibeShield
#### [2026-06-30 14:23] vibeshield:plan-check OK
**Sub-tarefa:** 4.2
**Stack detectado:** Node.js (a confirmar) / Express ou similar
**Modo:** pré-build, leigo

Achados:
- 🔴 C3 BLOQUEAR — Segredo GOOGLE_CLIENT_SECRET precisa ser guardada em 
  lugar seguro (.env). Decisão Tipo 1 a tomar.
- 🔴 C7 BLOQUEAR — HTTPS obrigatório em produção. Sem ação agora 
  (assumido pela hospedagem).
- 🟡 C1 REVISAR — MFA não é necessário pro caso (uso pessoal), mas 
  recomendado pra publicação pública.
- 🟡 C5 REVISAR — Token de sessão do Google pode precisar de cuidado 
  extra se persistido.
- 🟢 C8 OK — Log de tentativas de login será incluído no critério 
  de "pronto".

Decisões tomadas:
- Opção 1 — arquivo `.env` para GOOGLE_CLIENT_SECRET.

---
**Próxima ação para VibeDev**: gerar plano com 3 opções pra Tipo 1 
de C3 (já resolvido) + critério de "pronto" incluindo log de login.
**Categorias afetadas**: C1, C3, C5, C7, C8

---

#### Decisão (Tipo 1)
- **Decisão:** Usar `.env` para GOOGLE_CLIENT_SECRET
- **Racional:** padrão da indústria, simples, gratuito
- **Aprovado por:** usuário (2026-06-30 14:25)
- **Red Team:** [aplicado se necessário]
```

---

## Fase 6 — VibeDev monta o plano final

Baseado no risk register + decisão do usuário, VibeDev monta o `/vd-plan`:

```md
## Plano — 4.2 Adicionar login com Google OAuth

**Baseado em:** Auditoria VibeShield OK em 2026-06-30 14:23.

### Sub-tarefas (em ordem):

1. **Configurar Google Cloud Console** [🟢 ~15min]
   - Criar projeto, habilitar OAuth consent screen
   - Gerar `GOOGLE_CLIENT_ID` e `GOOGLE_CLIENT_SECRET`
   - Configurar redirect URI

2. **Criar arquivo `.env`** [🟢 ~5min]
   - Adicionar `.env` ao `.gitignore`
   - Documentar `.env.example` (sem secrets) para o repo

3. **Implementar fluxo OAuth no backend** [🔴 ~2h]
   - Type 1: vai precisar escolher lib de OAuth?
     - Opção A: Implementar manualmente (didático, mais código)
     - Opção B: Usar Passport.js (padrão Node.js)
     - Opção C: Usar Auth0/Clerk/Supabase Auth (gerenciado, mais simples)
   - Recomendação: Opção B (Passport.js) pela simplicidade

4. **Implementar frontend (botão "Entrar com Google")** [🟡 ~1h]

5. **Adicionar log de tentativas de login** [🟢 ~15min]

6. **Configurar sessão persistente** [🟡 ~1h]

### Critério de "pronto":
- [ ] Usuário clica "Entrar com Google" → tela de consentimento do Google
- [ ] Após consentir, redireciona pro app já logado
- [ ] Sessão persiste entre reloads do browser
- [ ] Botão "Sair" limpa sessão e volta pra landing
- [ ] Tentativas falhas ficam logadas com timestamp

**Posso executar este plano?**
```

---

## Fase 7 — Usuário aprova, VibeShield é acionada de novo em G5

Quando VibeDev entra em `/vd-build` e adiciona a dependência `passport-google-oauth20` ao `package.json`, **gatilho G5 dispara**:

```md
🔐 **Biblioteca nova: passport-google-oauth20**

Boa, mais uma peça no tabuleiro. Antes de seguir, deixa eu te explicar 
uma coisa rápida sobre bibliotecas:

Quando você pega código de outras pessoas (uma "biblioteca"), você tá 
confiando que ela é segura. Bibliotecas abandonadas há muito tempo 
são alvos fáceis pra invasores.

A gente checa 3 coisas:

✅ **Mantenedor ativo?** Sim — última release há 3 meses.
✅ **CVEs públicos conhecidos?** Vou te recomendar rodar `npm audit` 
no seu terminal pra confirmar 100%. Eu não executo direto.
💰 **Licença compatível?** Sim — MIT (uso comercial livre).

**Verdict:** OK

Dica: roda `npm audit --json | grep passport-google` no seu terminal 
e me mostra o resultado. Se aparecer algum problema de segurança, 
a gente troca de biblioteca.
```

---

## Fase 8 — `/vd-check` valida

Quando o usuário diz "tá pronto", VibeDev entra em check, dispara `vibeshield:postmortem` modo read-only se quiser panorama final:

```md
┌────────────────────────────────────────────────┐
│ 🛡️ Resumo do seu projeto — 2026-06-30         │
└────────────────────────────────────────────────┘

👤 Pra quem é seu app?
   Uso pessoal + amigos próximos.

📊 Quantas pessoas vão usar?
   ~10 nos primeiros 3 meses.

───────────────────────────────
   👍 O que tá bem
───────────────────────────────
   ✅ Login com Google funcionando
   ✅ Senha do Google guardada em arquivo 
      separado (.env)
   ✅ HTTPS configurado pela hospedagem
   ✅ Tentativas falhas são registradas

───────────────────────────────
   ⚠️ O que pediria atenção
───────────────────────────────
   🟡 MFA não tá ativado. Tudo bem pra uso 
      pessoal, mas confere antes de abrir pra 
      público.

───────────────────────────────
   🚨 O que pode dar problema se 
   publicar agora
───────────────────────────────
   (Nada bloqueante — pode publicar entre 
   amigos.)

🤖 Resumo em uma frase:

   "Tá pronto pra você e seus amigos usarem. 
   Quando for abrir pra mais gente, ativa MFA 
   primeiro."

❓ Quer que eu te ajude a resolver algo, ou 
   prefere seguir construindo?
```

---

## Comparação: como seria em Modo Técnico

Mesmo cenário, mas o usuário é dev sênior. No lugar do painel visual, VibeShield devolveria:

```md
### [2026-06-30 14:23] vibeshield:plan-check BLOQUEAR

**Sub-tarefa**: 4.2 — Adicionar login com Google OAuth
**Stack detectado**: Node.js / Express
**Modo**: pré-build, tecnico

#### 🔴 Bloqueante
- [ID-SEC-01] **C3** Segredo GOOGLE_CLIENT_SECRET precisa ser 
  armazenado em arquivo `.env` ou vault. Não pode estar hardcoded 
  nem em variável visível no cliente.
  Padrão: OWASP A02:2021 Cryptographic Failures / Secrets leakage
  Recomendação: usar `.env` + `dotenv` ou vault gerenciado.

- [ID-SEC-02] **C7** Callback OAuth do Google exige HTTPS. 
  Configurar TLS no deploy (Let's Encrypt ou CDN nativa).
  Padrão: OWASP A05:2021 Security Misconfiguration
  Recomendação: Vercel/Netlify/Railway cuidam disso automaticamente; 
  verificar antes do deploy.

#### 🟡 Requer decisão Tipo 1
- [ID-SEC-03] **C1** Decidir se MFA é necessário para este app 
  (uso pessoal = não agora; publicação pública = sim).
- [ID-SEC-04] **C5** Token JWT/Passport session: armazenamento 
  server-side vs JWT stateless. Decisão arquitetural.

#### 🟢 Aceitável
- [ID-SEC-05] **C8** Logging de tentativas falhas será incluído 
  automaticamente via `morgan` + middleware custom.

#### ⚪ Não auditado
- C2, C4, C6: dependem de stack não confirmada.

---
**Próxima ação para VibeDev**: bloquear /vd-build até Tipo 1 com 
3 opções para C3 + C7 já resolvidos, e C1/C5 como REVISAR.
**Categorias afetadas**: C1, C3, C5, C7, C8
```

Mesma informação, **renderização diferente**. O conteúdo escrito no `PROJECT_STATE.md` é idêntico.

---

## O que esse exemplo prova

| Aspecto | Demonstrado |
|---|---|
| **Fluxo end-to-end** | `/vd-plan` → gatilho detectado → VibeShield → decisão → `/vd-build` → G5 → check |
| **Modo Leigo funciona** | Painel visual + decisões com 3 opções + recomendação explícita |
| **Modo Técnico funciona** | Risk register formal com C1-C8 + verdict |
| **Estado coerente** | Mesmo conteúdo, dois formatos de saída |
| **Antifrágil a tipagem de erro** | Usuário podia ter escolhido opção 3 (colar no código); VibeShield teria registrado o risco e o framework teria seguido |
| **3 minutos por turno** | Cada bloco individual cabe em 3 minutos de leitura |
| **Decision Log preenchido** | Decisão Tipo 1 (C3) foi pro log automaticamente |

---

## Como reusar este exemplo

1. **Ler uma vez** pra entender o fluxo.
2. **Não copiar texto** — cada projeto tem contexto.
3. **Usar como template mental**: "se eu tivesse que adicionar OAuth no meu projeto, o framework me mostraria X, Y, Z".
4. **Treinar colegas**: "olha como a VibeShield te guia numa decisão que parece complexa mas vira 3 opções claras".

---

**Versão**: 1.0.0  
**Stack exemplo**: Node.js / Express  
**Modo**: ambos (Técnico + Leigo paralelos)  
**Categorias exercitadas**: C1, C3, C5, C7, C8  
**Gatilhos exercitados**: G1, G3, G5
