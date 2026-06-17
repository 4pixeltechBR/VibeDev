# Trilha Verde — Greenfield (Projeto Novo)

Usado quando: zero código existente, começando do zero.
Risco principal: construir a coisa errada na sequência errada.
Antídoto: validar antes de construir, uma fase por vez, gates obrigatórios.

---

## As 8 Fases

### FASE 1 — Validação
**Objetivo:** confirmar que o problema é real antes de escrever uma linha.

O que fazer:
- Descrever o problema em 1 frase do ponto de vista de quem sofre com ele
- Identificar pelo menos 1 evidência externa (conversa com usuário-alvo,
  dado de mercado, dor própria documentada e recorrente)
- Definir critério de sucesso: "o problema está resolvido quando..."
- Definir kill criteria: "abandono o projeto se..."
- Decidir conscientemente: vale construir vs resolver manualmente / no-code?

Protótipo descartável de frontend é permitido NESTA FASE para validar
demanda — deve ser marcado como descartável no estado e NUNCA promovido
a código de produção.

Gate para Fase 2: leia `references/gates.md` seção Gate 1→2.

---

### FASE 2 — Especificação
**Objetivo:** definir o escopo exato — o que entra E o que não entra.

O que fazer:
- Listar as funcionalidades do MVP em no máximo 10 itens, cada uma testável
- Escrever o anti-escopo: o que conscientemente NÃO entra na v1
- Nomear as entidades principais de dados e seus relacionamentos
  (ex: "um Usuário tem muitos Pedidos; um Pedido tem um Status")
- Descrever o fluxo principal do usuário ponta a ponta em texto simples

Por que isso importa: sem anti-escopo, o projeto cresce sem parar.
Sem fluxo descrito, a arquitetura fica sem direção.

Gate para Fase 3: leia `references/gates.md` seção Gate 2→3.

---

### FASE 3 — Arquitetura
**Objetivo:** decidir a stack e o modelo de dados. Decisões Tipo 1.

O que fazer:
- Escolher linguagem + framework (Tipo 1 → 3 opções + Red Team)
- Escolher banco de dados (Tipo 1 → 3 opções + Red Team)
- Desenhar o modelo de dados e revisar contra o fluxo principal da Fase 2
- Definir contratos entre camadas (o que cada parte do sistema recebe
  e devolve — mesmo que em texto simples)
- Registrar pre-mortem: "se a arquitetura falhar em 6 meses, por quê?"

Recomendações padrão para vibe coders (ajuste conforme restrições):
- App web simples: Python/FastAPI + PostgreSQL ou SQLite
- Bot/agente: Python + SQLite + biblioteca do canal (Telegram, WhatsApp)
- Site/landing: HTML/CSS estático ou Next.js se precisar de backend
Consulte `references/stack-guide.md` para casos específicos.

Gate para Fase 4: leia `references/gates.md` seção Gate 3→4.

---

### FASE 4 — Segurança & Dados
**Objetivo:** proteger o projeto antes de construir. Não é opcional.

O que fazer:
- Decidir estratégia de autenticação (Tipo 1 — como usuários fazem login)
- Garantir que segredos (senhas, chaves de API) fiquem fora do código
  em arquivo .env (nunca no git)
- Mapear dados pessoais coletados e avaliar LGPD
- Definir estratégia de backup do banco
- Adicionar `.env` e pastas de dados ao `.gitignore`

Por que antes do frontend: segurança retrofitada é 10x mais cara e
10x mais frágil do que segurança planejada desde o início.

Gate para Fase 5: leia `references/gates.md` seção Gate 4→5.

---

### FASE 5 — Versionamento & Estrutura
**Objetivo:** criar a fundação técnica do projeto.

O que fazer:
- Inicializar repositório git e fazer o primeiro commit
- Criar estrutura de pastas (documentar o porquê de cada pasta)
- Fixar versões das dependências (requirements.txt, package.json)
- Criar ambiente reproduzível (venv Python, node_modules, container)
- Testar: um colega consegue rodar do zero seguindo só o README?

Este é o momento de criar o README inicial com: o que é, como instalar,
como rodar, variáveis de ambiente necessárias.

Gate para Fase 6: leia `references/gates.md` seção Gate 5→6.

---

### FASE 6 — Construção com Observabilidade
**Objetivo:** construir backend → frontend, com logs desde o 1º endpoint.

Regra de ouro: lógica e dados antes de interface.

O que fazer:
- Construir endpoints/funcionalidades na ordem do fluxo principal
- Primeiro endpoint já deve ter: log de entrada, tratamento de erro,
  resposta padronizada
- Testar cada endpoint antes de construir o próximo
- **Teste mínimo:** ao concluir o fluxo principal, escreva pelo menos
  1 teste automatizado cobrindo o happy path. Pode ser simples (requests
  ao endpoint, assert no status) — o importante é que exista e rode.
- Frontend só começa quando o backend do fluxo principal está funcionando
- Backlog revisado a cada sub-tarefa: nada crítico esquecido, nada
  fora de escopo vazou para o código

Por que logs desde o início: debugar sem logs é trabalhar no escuro.
Por que backend primeiro: interface bonita em cima de lógica quebrada
cria a ilusão de progresso sem progresso real.

Gate para Fase 7: leia `references/gates.md` seção Gate 6→7.

---

### FASE 7 — Homologação & Lançamento
**Objetivo:** validar com uso real antes de considerar lançado.

O que fazer:
- Usuário real (ou o builder em papel de usuário ingênuo) completa o
  fluxo principal SEM instrução verbal — só o produto guia
- Documentar deploy reproduzível: passo a passo para colocar em produção
  incluindo rollback se algo der errado
- Estimar custos de operação e comparar com kill criteria
- Corrigir tudo que o teste de uso real revelou

O critério mais honesto: você consegue dar o produto para alguém que
nunca viu e ele consegue usar sem perguntar nada?

Gate para Fase 8: leia `references/gates.md` seção Gate 7→8.

---

### FASE 8 — Operação
**Objetivo:** manter o projeto saudável após o lançamento.

O que fazer:
- Configurar monitoramento básico (o que alertar, como alertar)
- Definir rotina de revisão de custos
- Registrar post-mortem do projeto inteiro no estado
- Iniciar ciclo de melhoria: coletar feedback → priorizar → planejar

A Fase 8 não tem gate de saída — é um ciclo contínuo.
Novas funcionalidades significativas voltam para a Fase 2 (Especificação).
