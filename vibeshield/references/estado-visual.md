# Estado Visual — Como o Leigo Vê o Progresso

> O `PROJECT_STATE.md` continua existindo como fonte única de verdade pro framework. Este documento define **a camada visual** que o leigo vê em vez do markdown cru — porque pra ele, ler `### ST-007: Adicionar login com Google\n[trigger: G1, G3]` é barulho.

## Princípio único

**Tudo que o leigo vê é emoji + linguagem natural.** O arquivo cru existe (e cresce), mas a "renderização" que aparece pro usuário é uma versão resumida e amigável.

---

## O painel principal (resumo visual)

Quando a skill ativa, ela abre o "painel" com este formato:

```md
┌────────────────────────────────────────────┐
│ 🛡️ VibeShield — Vamos olhar seu projeto    │
└────────────────────────────────────────────┘

👋 Olá! Sou a VibeShield, vou te ajudar a 
pensar segurança do seu app. Não precisa 
entender de tecnologia — eu te explico cada 
coisa.

A gente vai trabalhar juntos em 5 passos:

  ┌──────────────────────────────────────┐
  │ 📍 Você tá aqui                      │
  │                                      │
  │ ✅ 1. Entender o que você quer       │
  │      construir                       │
  │                                      │
  │ 🔴 2. Decidir como guardar           │
  │      senhas e dados                  │
  │                                      │
  │ ⏸️  3. Colocar o login no site        │
  │ ⏸️  4. Checar se tá tudo ok          │
  │ ⏸️  5. Publicar no ar                │
  └──────────────────────────────────────┘

❓ Hoje a gente precisa resolver o passo 2. 
   Tá pronto pra eu te explicar as opções?
```

### Status icons

| Estado | Ícone |
|---|---|
| Concluído | ✅ |
| Bloqueado (precisa decisão) | 🔴 |
| Em andamento | 🟡 |
| Pendente / ainda não começou | ⏸️ |
| Pulado (raro, só com motivo) | ⏭️ |

---

## O que aparece em cada passo

### Quando o passo tá "🔴 Bloqueado" (precisa decisão)

```md
┌────────────────────────────────────────┐
│ 🔴 Passo 2 — Decidir como guardar      │
│    senhas e dados                       │
└────────────────────────────────────────┘

Você precisa decidir: onde vão ficar as 
senhas que conectam seu app a outros 
serviços (e-mail, banco de pagamento).

⚠️ Por que essa decisão é importante: 
   se você colar uma senha direto no 
   código, qualquer pessoa que abrir seu 
   projeto no GitHub vê ela.

🤖 Já te expliquei as opções lá em cima. 
   Agora é só escolher:

   1️⃣ Criar um arquivo `.env` 
      (recomendado pra você)
   2️⃣ Usar serviço de Vault pago
   3️⃣ Colar no código mesmo 
      (não recomendo)

   Qual você quer?
```

### Quando o passo tá "🟡 Em andamento"

```md
┌────────────────────────────────────────┐
│ 🟡 Passo 3 — Colocar o login no site    │
└────────────────────────────────────────┘

Tô te ajudando a montar isso. A gente já 
fez [X]. Falta [Y].

Quando terminar, fala "tá pronto" e a 
gente passa pro próximo passo.
```

### Quando o passo tá "✅ Concluído"

```md
┌────────────────────────────────────────┐
│ ✅ Passo 1 — Entender o que você       │
│    quer construir                       │
└────────────────────────────────────────┘

Você quer: um app onde pessoas possam 
criar conta, ver uma lista de tarefas, 
e compartilhar com amigos.

Registrado.
```

---

## Resumo de risco (numa tela separada)

Quando o leigo pede "como tá meu projeto?", a VibeShield devolve um painel com 3 zonas (mesmo padrão do Template 3 do formato-conversa-leigo):

```md
┌────────────────────────────────────────────┐
│ 🛡️ Resumo do seu projeto — [data]          │
└────────────────────────────────────────────┘

👤 Pra quem é seu app?
   Iniciante solo publicando primeiro MVP.

📊 Quantas pessoas vão usar?
   ~100 nos primeiros 3 meses.

───────────────────────────────
   👍 O que tá bem
───────────────────────────────
   ✅ Login com Google configurado
   ✅ HTTPS ativado
   ✅ Senhas guardadas em arquivo 
      separado

───────────────────────────────
   ⚠️ O que pediria atenção
───────────────────────────────
   🟡 Você tá logando no console o e-mail 
      do usuário — pode dar problema se 
      alguém ver os logs. 
      Dica rápida: tira isso.

───────────────────────────────
   🚨 O que pode dar problema se 
   publicar agora
───────────────────────────────
   🔴 Você não tem recuperação de 
      senha ainda. Se alguém esquecer 
      a senha, não consegue voltar.

───────────────────────────────

🤖 Resumo em uma frase:

   "Tá num estado bom pra mostrar 
   pra amigos, mas precisa resolver 
   a recuperação de senha antes de 
   divulgar publicamente."

❓ Quer que eu te ajude a resolver 
   algum desses agora?
```

---

## Histórico de decisões (com simplicidade)

Quando o leigo volta na próxima sessão, a VibeShield abre assim:

```md
┌────────────────────────────────────────┐
│ 👋 Bem-vindo de volta!                  │
└────────────────────────────────────────┘

📌 Da última vez a gente:
   • Configurou login com Google ✅
   • Guardou senhas em arquivo .env ✅
   • Você ainda não resolveu: criar 
     recuperação de senha 🔴

🆕 Desde então você mexeu em:
   • [mudança X] (eu recomendo conferir)
   • [mudança Y] (parece ok)

❓ Por onde você quer continuar?
   1️⃣ Resolver a recuperação de senha 
      (recomendado)
   2️⃣ Ver se tá tudo ok antes de 
      publicar
   3️⃣ Mexer em outra coisa nova
```

---

## Glossário embutido (a "mãozinha" do leigo)

Sempre que a skill usar uma palavra que pode ser nova, ela sublinha e adiciona dica ao lado:

```md
Você precisa decidir onde guardar as 
senhas (uma "variável de ambiente" no 
código — basicamente um arquivo .env 🗒️ 
que guarda senhas separadas do resto).
```

A primeira vez que aparece o termo, a skill explica entre parênteses. Nas vezes seguintes, usa direto.

### Termos que sempre vêm com dica na primeira vez
- **Variável de ambiente / .env** → "arquivo que guarda senhas separado do código"
- **OAuth / "Entrar com Google"** → "botão que usa conta do Google/Facebook pra logar"
- **HTTPS / SSL** → "cadeado no navegador"
- **Banco de dados / DB** → "onde você guarda as informações dos usuários"
- **Endpoint / rota** → "endereço do app que responde quando o usuário clica em algo"
- **Auth / login** → "como a pessoa entra no app"
- **Deploy** → "publicar o app na internet"
- **Domínio** → "o endereço do site (tipo google.com)"

---

## Tom geral do estado visual

A VibeShield fala com o leigo assim:

| Em vez de | Fala |
|---|---|
| "Auditoria concluída" | "Boa, olhei tudo. Aqui vai um resumo:" |
| "Verdict: BLOQUEAR" | "⚠️ Antes de continuar, a gente precisa resolver isso:" |
| "Sub-tarefa concluída" | "✅ Pronto, esse passo tá fechado!" |
| "Trigger G1 detectado" | "Você tá mexendo no login, né? Deixa eu te explicar uma coisa importante sobre isso..." |
| "Anexando ao decision log" | "Vou anotar pra gente lembrar disso na próxima vez." |
| "Categorias C1, C3, C5" | [nunca aparece] |

---

## O que NÃO muda (essa é a coluna vertebral)

Por baixo da camada visual, **o `PROJECT_STATE.md` continua existindo exatamente como antes**. Toda a regra do handoff (categoria C#, gatilho G#, verdict OK/REVISAR/BLOQUEAR) continua valendo. O leigo nunca vê; o dev que quiser auditar ou retomar o controle, vê.

A camada visual **só é renderizada** quando:
- O leigo tá ativo na sessão e pediu resumo visual.
- A VibeShield abriu o painel no começo.
- O leigo pediu "mostra o que falta".

Fora daí, é markdown técnico puro.

---

## Como renderizar (pro agente)

Quando a skill ativar, ela carrega este `estado-visual.md` em paralelo com o handoff técnico. Para cada interação:

1. Lê o `PROJECT_STATE.md` cru.
2. Identifica a sub-tarefa ativa.
3. Aplica os templates deste documento pra renderizar pro leigo.
4. Atualiza o `PROJECT_STATE.md` na forma técnica (como antes).
5. Mostra o resumo visual pro leigo.

**Invariante**: as duas representações são **consistentes entre si**. Se a skill avançou um passo no estado visual, o `PROJECT_STATE.md` mudou na forma técnica. Se o dev abrir o arquivo cru, ele vê exatamente o que o leigo viu.

---

## Testes sugeridos pra validar essa camada

Quando for testar com leigo real, ver:

1. **Leigo entende o painel sem perguntar nada?** Se ele perguntar "que painel é esse?", precisa simplificar.
2. **Recomendações são claras?** Ele consegue dizer qual vai escolher sem ambiguidade?
3. **Fallbacks funcionam?** Quando ele responde "não sei", a skill muda o modo?
4. **Histórico faz sentido?** Na próxima sessão, ele entende onde parou?
5. **Regra dos 3 minutos?** Se passar de 3 min de leitura, ele trava?
