# Glossário Leigo — Tradução de C1-C8 pra Linguagem Natural

> Este documento é a **camada de tradução** entre as categorias técnicas internas da VibeShield (C1-C8) e a linguagem que o leigo vê. O framework continua rigoroso por dentro; o leigo conversa por fora.

## Como usar este glossário

A skill carrega este dicionário em memória durante toda a sessão. Quando uma análise interna detecta um risco em C3 (Segredos & Credenciais), ela **não expõe "C3" pro leigo** — em vez disso, dispara a frase de pergunta correspondente (abaixo), reformulada pro contexto do que o usuário está fazendo.

**Regra de ouro**: o leigo **nunca** vê o ID da categoria (C1, C2 etc.). Vê só a pergunta + 3 opções com recomendação.

---

## C1 — Autenticação & Sessão

### Pergunta de abertura
**"Como as pessoas vão entrar no seu app?"** (login, criar conta, "entrar com Google", "esqueci minha senha")

### Frases-âncora (a skill escolhe conforme o contexto)
- "Boa, antes de mexer no login, deixa eu te explicar uma coisa importante…"
- "Você já pensou em como a pessoa vai **lembrar que tá logada** quando voltar amanhã?"
- "E se alguém esquecer a senha, como ela recupera?"

### O que a skill faz por baixo
- Procura string no código: `login`, `password`, `token`, `jwt`, `oauth`, `session`, `cookie`, `clerk`, `supabase.auth`, `firebase.auth`, `auth0`, `nextauth`.
- Se não houver nenhum mecanismo identificável: marca como **bloqueante** ("não tem login ainda").
- Se houver mas faltar MFA / recuperação de senha / logout: marca como **revisar**.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Usar login com Google/Facebook** | "Aperta um botão, o Google confirma quem é a pessoa. Gratuito, simples. Você não guarda senha nenhuma." | **Recomendado na maioria dos casos** — é o caminho mais simples pro leigo. |
| **2 — Login com e-mail + senha** | "A pessoa cria conta com e-mail e senha. Você guarda a senha (criptografada) no seu banco." | Só se o usuário precisar de público que não tem Google/Facebook (raro). |
| **3 — Pedir código por SMS / e-mail** | "A pessoa digita o e-mail, recebe um código, entra. Sem senha." | Bom pra apps com pouca frequência de uso. Recomendado se o público tem medo de Google. |

### Linguagem que NÃO usar
- ❌ "OAuth 2.0", "JWT token", "session cookie", "refresh token".
- ✅ "Botão de entrar com Google", "a pessoa fica logada por X tempo", "lembrar do usuário".

---

## C2 — Autorização & Permissão

### Pergunta de abertura
**"E como você garante que uma pessoa não vê os dados de outra?"**

### Frases-âncora
- "Boa, agora pensa: se você tem dois usuários, o João não pode ver o que a Maria cadastrou, né?"
- "Cada pessoa só vê o que é dela. Como você tá separando isso?"

### O que a skill faz por baixo
- Procura endpoints/mutations que recebem ID/ID-parâmetro direto sem checar `owner_id`.
- Procura padrões de "todos podem ver tudo" (lista sem filtro por usuário).
- Procura APIs admin sem proteção.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Cada coisa só é vista por quem criou** | "Quando alguém cadastra algo, você grava de quem é. Aí antes de mostrar, você confere se é daquela pessoa." | **Recomendado pra 90% dos apps** (MVP). |
| **2 — Níveis de usuário (admin, usuário comum)** | "Tem gente que pode tudo (admin), e gente que só vê o próprio (comum)." | Se o app tem staff, moderadores, ou painel admin. |
| **3 — Compartilhamento explícito** | "Cada coisa tem botões 'compartilhar com fulano'. Tipo Google Drive." | Apps colaborativos (Notion, Figma). Mais complexo. |

### Linguagem que NÃO usar
- ❌ "RBAC", "ACL", "row-level security", "IDOR", "broken access control".
- ✅ "Quem pode ver o quê", "separar por dono", "quem é admin".

---

## C3 — Segredos & Credenciais

### Pergunta de abertura
**"Você tá colando senhas de serviço (e-mail, banco, Stripe) dentro do código, ou tá guardando em outro lugar?"**

### Frases-âncora
- "Pensa assim: as senhas que conectam seu app a outros serviços (e-mail, banco, Stripe) precisam ficar **fora do código**. Senão, qualquer pessoa que abrir seu projeto vê tudo."
- "Você já ouviu falar de arquivo `.env`? É um arquivo que não vai pro GitHub e guarda essas senhas separadas."

### O que a skill faz por baixo
- Procura padrões: `process.env.X`, mas com valor default hardcoded (ex: `process.env.STRIPE_SECRET || "sk_live_..."`).
- Procura string em código que pareça API key (`sk_live_`, `ghp_`, `AKIA`, `xoxb-`).
- Confere se `.env` está no `.gitignore`.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Guardar em arquivo `.env`** | "Cria um arquivo `.env` na raiz do projeto, escreve a senha lá. O código lê de lá. Esse arquivo nunca vai pro GitHub porque tem um arquivo `.gitignore` que protege." | **Recomendado pra começar.** Padrão da indústria, gratuito. |
| **2 — Usar serviço deVault** | "Serviços como Doppler, Infisical ou Vault da Cloud (pagos ou grátis no começo) guardam suas senhas em nuvem." | Quando você tem várias senhas e trabalha em time. |
| **3 — Colar no código mesmo** | "Funciona, mas qualquer pessoa que abrir seu projeto vê a senha." | **Não recomendo. Só pra teste local descartável.** |

### Linguagem que NÃO usar
- ❌ "Environment variables", "secrets manager", "credential leakage".
- ✅ "Senha guardada em lugar separado do código", "arquivo que não vai pro GitHub".

---

## C4 — Validação & Sanitização

### Pergunta de abertura
**"Se um usuário digitar besteira (tipo um emoji no campo de nome, ou um texto gigante), seu app aguenta ou quebra?"**

### Frases-âncora
- "Imagina: alguém digita `'<script>roubar(' no campo de nome. O que acontece?"
- "Você tá confiando que o usuário vai digitar certinho. Spoiler: não vai."

### O que a skill faz por baixo
- Procura string concatenada em query SQL (em vez de parametrizada).
- Procura renderização de texto direto em HTML (`dangerouslySetInnerHTML`, `innerHTML`, `v-html`).
- Procura `eval`, `exec`, `child_process.exec` com input do usuário.
- Procura falta de validação de tamanho/tipo nos endpoints.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Validar na entrada do dado** | "Você confere se o campo é do tipo certo e tem tamanho razoável antes de salvar." | **Sempre.** Leigo deveria usar bibliotecas de validação prontas (Zod, Joi, Yup). |
| **2 — Usar ORM ou queries parametrizadas** | "Ferramentas tipo Prisma escrevem SQL seguro por padrão. Você não digita SQL diretamente." | **Recomendado se usa banco SQL.** |
| **3 — Escapar todo texto antes de mostrar** | "Quando o texto do usuário vai aparecer no HTML, você 'escapa' ele (transforma `<` em `&lt;`, por exemplo)." | Automático na maioria dos frameworks modernos — só se preocupa se usa `innerHTML` direto. |

### Linguagem que NÃO usar
- ❌ "SQL injection", "XSS", "command injection", "path traversal", "sanitização de input".
- ✅ "Validar o que o usuário digitou", "texto que pode quebrar o app", "campo com regra".

---

## C5 — Criptografia & Dados Sensíveis

### Pergunta de abertura
**"Você tá guardando coisas como CPF, RG, cartão de crédito, dados de saúde? Tá tudo trancado?"**

### Frases-âncora
- "Se você guarda dado sensível (CPF, cartão, prontuário), você tem **obrigação legal** de cuidar bem dele."
- "Senha no banco de dados não pode ser a senha real — tem que ser um código dela. Senão, alguém que abrir seu banco vê tudo."

### O que a skill faz por baixo
- Procura campos com nome sensível em schema: `cpf`, `rg`, `card`, `credit_card`, `password`, `secret`.
- Confere se tá usando bcrypt/argon2 (nunca MD5/SHA1).
- Procura PII em logs ou em respostas HTTP.
- Confere TLS (HTTPS) na porta de produção.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Hash de senha (bcrypt)** | "Você usa uma biblioteca pronta que transforma a senha num código que ninguém consegue reverter. A pessoa só sabe sua senha, nem você." | **Sempre, obrigatório.** |
| **2 — HTTPS no site** | "Aquele cadeado no navegador. Sem isso, tudo que trafega é legível por quem tá na rede. Let's Encrypt dá certificado grátis." | **Sempre em produção.** |
| **3 — Criptografar dados em repouso (banco)** | "Até o banco de dados, o dado tá criptografado. Se vazarem o banco, ainda tá trancado." | Pra dados muito sensíveis (saúde, financeiro). |

### Linguagem que NÃO usar
- ❌ "bcrypt", "Argon2", "criptografia simétrica/assimétrica", "PII", "PCI-DSS".
- ✅ "Senha trancada com código", "cadeado no navegador", "dados protegidos mesmo se o banco vazar".

---

## C6 — Dependências & Supply Chain

### Pergunta de abertura
**"Quando você copiou aquele código de alguém (uma 'biblioteca' ou 'dependência'), você verificou se ela é de alguém confiável e se tá atualizada?"**

### Frases-âncora
- "Você provavelmente tá usando dezenas de códigos que outras pessoas escreveram (bibliotecas). Cada uma delas é uma janela pro seu app. Se uma tiver problema, todo o seu app tem problema."
- "Bibliotecas desatualizadas há muito tempo são alvos fáceis pra invasores."

### O que a skill faz por baixo
- Lê `package.json`, `requirements.txt`, etc.
- Checa idade da última release (se > 12 meses sem release, alerta).
- Procura por CVEs conhecidos usando `npm audit`, `osv.dev`, `pip-audit`, etc. Recomenda **rodar o comando**, ela própria não executa.
- Checa licenças incompatíveis (AGPL em app comercial é problemático).

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Atualizar tudo agora** | "Roda `npm update` ou equivalente. Resolve muita coisa de uma vez." | **Recomendado na dúvida.** |
| **2 — Trocar por biblioteca equivalente mais saudável** | "Essa biblioteca tá abandonada há 2 anos, troca por uma ativa com manutenção." | Quando a biblioteca tá claramente morrendo. |
| **3 — Ficar com a versão desatualizada mas travar** | "Atualizar quebra coisa. Vamos travar a versão atual e planejar migração." | Raro, só quando atualizar causa problema sério. |

### Linguagem que NÃO usar
- ❌ "CVE", "supply chain attack", "typosquatting", "transitive dependencies".
- ✅ "Biblioteca", "pacote", "atualizado", "abandonada".

---

## C7 — Configuração & Deploy

### Pergunta de abertura
**"Quando alguém usa seu app na internet, todas as travas de segurança tão ativas? Tem aquele cadeado no navegador?"**

### Frases-âncora
- "Sabe quando você esquece uma 'trava' e o app abre tudo? É tipo você sair de casa com a porta destrancada. Acontece muito."
- "Em desenvolvimento você liga modo 'debug' pra ver erros. Em produção, esquece de desligar, e os erros mostram senhas."

### O que a skill faz por baixo
- Procura `Access-Control-Allow-Origin: *` em apps autenticados.
- Procura `DEBUG = True` em settings de produção (Django).
- Procura `console.log` de tokens/secrets.
- Confere CSP, HSTS, X-Frame-Options.
- Confere se `helmet()` (Express) ou equivalente tá sendo usado.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Usar templates prontos** | "Frameworks como Next.js, Django, Rails já vêm com travas boas por padrão. Você só precisa não desligar." | **Sempre.** |
| **2 — Configurar HTTPS (cadeado)** | "Let's Encrypt te dá certificado grátis. Hospedagem moderna configura sozinha." | **Sempre em produção.** |
| **3 — Modo debug desligado em produção** | "Confere que a opção 'debug' tá desligada antes de publicar." | Checklist padrão antes do deploy. |

### Linguagem que NÃO usar
- ❌ "CORS", "CSP", "HSTS", "Helmet", "OWASP Misconfiguration", "security headers".
- ✅ "Trava de segurança", "cadeado no navegador", "modo debug desligado".

---

## C8 — Logging & Observabilidade

### Pergunta de abertura
**"Você consegue ver depois quem tentou entrar no app? Quem tentou senhas erradas? Quem acessou dados sigilosos?"**

### Frases-âncora
- "Se alguém tentar invadir seu app 500 vezes, você quer **saber**. Senão, você só descobre quando o estrago tá feito."
- "Logs de auditoria parecem chatos, mas são o que salva você quando precisa investigar um incidente."

### O que a skill faz por baixo
- Procura log de login attempts (success/fail).
- Procura log de ações sensíveis (view de dado pessoal, mudança de permissão).
- Procura PII em logs (ex: `console.log(user.cpf)`).
- Procura alertas de tentativas falhas sucessivas.

### 3 opções típicas a apresentar

| Opção | Linguagem | Quando recomendar |
|---|---|---|
| **1 — Logar tentativas de login** | "Cada tentativa de entrar (sucesso ou falha) fica registrada com data, IP." | **Sempre.** |
| **2 — Alerta quando tem muitas tentativas falhas** | "Se alguém errar senha 10 vezes em 5 minutos, você recebe um aviso." | Pra apps com login público (risco de brute force). |
| **3 — Nunca logar dado sensível** | "No log, você guarda só o necessário. CPF, cartão, senha JAMAIS no log." | **Sempre.** Padrão de hygiene. |

### Linguagem que NÃO usar
- ❌ "Audit log", "SIEM", "alerting", "PII em logs", "GDPR compliance".
- ✅ "Registro de quem fez o quê", "alerta quando algo estranho", "cuidado com o que você anota".

---

## Frases-âncora genéricas (qualquer categoria)

Frases que a skill usa independente da categoria quando o leigo trava ou desiste:

- **Quando o leigo não entende a pergunta**: "Deixa eu te explicar de outro jeito. Imagina que você tem uma lojinha e…"
- **Quando o leigo quer pular**: "Eu entendo que é muita coisa, mas essa trava aqui te protege de um problema real. Te explico o que acontece se você pular?"
- **Quando o leigo tá perdido**: "Então, recapitulando: a gente tá no passo X de Y. O próximo passo é decidir [pergunta curta]. Vou te dar 3 jeitos de fazer."
- **Quando o leigo fez algo arriscado**: "Boa, agora a gente vai resolver isso direitinho. Esse problema específico [1 frase explicando o risco em linguagem chã]."
- **Quando o leigo tá inseguro**: "Tudo bem não saber. Eu te explico cada opção e você escolhe. Sem pressa."

## Princípio unificador

Toda tradução preserva:
1. **O risco real** (sem mentir sobre a gravidade).
2. **A escolha do usuário** (sem decidir por ele).
3. **A simplicidade da explicação** (sem jargão).
4. **A opção recomendada explícita** (sempre 3, com uma destacada).
