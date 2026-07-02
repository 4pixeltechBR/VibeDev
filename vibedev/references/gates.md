# Gates de Transição entre Fases

Consulte este arquivo ao fechar uma fase. Um gate só passa com TODOS os itens
evidenciados — não afirmados. "Vou fazer isso" não conta. "Fiz e funcionou" conta.
Item impossível/não-aplicável → registrar o porquê no Decision Log e seguir.

---

## TRILHA VERDE

### Gate 1→2 (Validação → Especificação)
- [ ] Problema descrito em 1 frase do ponto de vista de quem sofre com ele
- [ ] Pelo menos 1 evidência externa documentada (conversa, dado, dor própria)
- [ ] Critério de sucesso registrado no estado
- [ ] Kill criteria registrado no estado
- [ ] Decisão consciente: vale construir (não resolver manualmente/no-code)
- [ ] Protótipo descartável (se houver) marcado como descartável no estado

### Gate 2→3 (Especificação → Arquitetura)
- [ ] MVP em ≤10 funcionalidades, cada uma testável individualmente
- [ ] Anti-escopo escrito: o que conscientemente NÃO entra na v1
- [ ] Entidades de dados principais nomeadas com seus relacionamentos
- [ ] Fluxo principal do usuário descrito ponta a ponta em texto

### Gate 3→4 (Arquitetura → Segurança)
- [ ] Stack decidida via processo Tipo 1 (3 opções + Red Team registrado no log)
- [ ] Banco de dados escolhido via processo Tipo 1
- [ ] Modelo de dados desenhado e revisado contra o fluxo principal
- [ ] Pre-mortem registrado: "se a arquitetura falhar em 6 meses, por quê?"

### Gate 4→5 (Segurança → Versionamento)
- [ ] Estratégia de autenticação decidida (Tipo 1) e registrada
- [ ] Segredos fora do código — verificado, não prometido
- [ ] `.env` e dados sensíveis no `.gitignore` — arquivo existe e testado
- [ ] Dados pessoais mapeados; LGPD avaliada
- [ ] Estratégia de backup do banco definida

### Gate 5→6 (Versionamento → Construção)
- [ ] Repositório git iniciado, primeiro commit feito
- [ ] Estrutura de pastas criada e documentada
- [ ] Dependências fixadas com versões (requirements.txt ou package.json)
- [ ] Ambiente reproduzível testado: zero para rodando seguindo só o README

### Gate 6→7 (Construção → Homologação)
- [ ] Todos os endpoints/fluxos do MVP com teste comportamental aprovado
- [ ] Pelo menos 1 teste automatizado cobrindo o happy path do fluxo principal
- [ ] Logs e tratamento de erro presentes desde o primeiro endpoint
- [ ] Nenhuma falha aberta no estado
- [ ] Backlog revisado: nada crítico esquecido, nada fora de escopo no código

### Gate 7→8 (Homologação → Operação)
- [ ] Usuário real completou o fluxo principal sem instrução verbal
- [ ] Deploy documentado passo a passo com rollback incluído
- [ ] Custos de operação estimados e comparados com kill criteria
- [ ] Post-mortem do projeto inteiro registrado no estado

---

## TRILHA VERMELHA

### Gate R1→R2 (Arqueologia → Triagem)
- [ ] Mapa do Caos escrito no PROJECT_STATE.md
- [ ] Linguagens e bibliotecas identificadas
- [ ] O que está em produção vs em desenvolvimento diferenciado
- [ ] Fluxo principal identificado
- [ ] Estado do git verificado

### Gate R2→R3 (Triagem → Estabilização)
- [ ] Todos os problemas encontrados classificados (P0/P1/P2/P3)
- [ ] Lista de Triagem revisada e aprovada pelo usuário
- [ ] P0s identificados com clareza

### Gate R3→R4 (Estabilização → Remediação)
- [ ] Git inicializado e commit do estado atual ("snapshot do caos") feito
- [ ] README mínimo criado (o que é, como instalar, como rodar)
- [ ] Segredos fora do código (P0 de segurança corrigido)
- [ ] Dependências documentadas

### Gate R4→R5 (Remediação → Documentação)
- [ ] Todos os P0s resolvidos e verificados com evidência
- [ ] P1s resolvidos ou com plano documentado e data estimada
- [ ] Nenhuma falha nova introduzida (verificado via `/vd-check`)
- [ ] Teste smoke cobrindo os P0 corrigidos, onde tecnicamente viável
- [ ] Commit feito após cada correção

### Gate R5→Verde (Graduação)
- [ ] README completo escrito
- [ ] Decisões de arquitetura documentadas no Decision Log
- [ ] Post-mortem da Trilha Vermelha registrado
- [ ] Fase de entrada na Trilha Verde definida e justificada

---

## Red Team padrão para decisões Tipo 1
Aplique à opção recomendada antes de apresentar ao usuário:

1. **Competência:** o usuário consegue manter e debugar isso sozinho em
   6 meses? (para vibe coders este é o critério eliminatório — stack que
   o dono não consegue manter sozinho é a causa #1 de morte de projetos)
2. **Quebra:** qual o modo de falha mais provável desta escolha?
3. **Suposição crítica:** o que estou assumindo que, se falso, derruba tudo?
4. **Escala:** com 10x volume/usuários, onde aperta primeiro?
5. **Lock-in:** quanto custa sair desta escolha daqui a 1 ano?
6. **Concentração:** dependo de um único provedor/conta/API que pode me
   banir ou mudar preço sem aviso?
