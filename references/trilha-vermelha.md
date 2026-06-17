# Trilha Vermelha — Rescue (Projeto Existente com Problemas)

Usado quando: código existente com problemas estruturais, arquitetura ruim,
sem documentação, ou qualquer combinação de "Frankenstein técnico".
Risco principal: quebrar o que funciona enquanto tenta consertar o que não funciona.
Antídoto: mapear antes de tocar, priorizar por risco, nunca refatorar sem backup.

**Regra de ouro da Trilha Vermelha:** entenda tudo antes de mudar qualquer coisa.

---

## As 5 Fases

### FASE R1 — Arqueologia
**Objetivo:** mapear o que existe sem tocar em nada.

O que fazer:
- Listar todos os arquivos e pastas com uma linha descrevendo o que cada um faz
- Identificar qual linguagem e quais bibliotecas estão sendo usadas
- Mapear o que está "em produção" (alguém usa) vs o que está em desenvolvimento
- Identificar o fluxo principal: qual é o caminho que um usuário real percorre?
- Documentar o que está funcionando e o que está quebrado (sem tentar consertar)
- Verificar se existe git inicializado e qual é o estado do repositório

Saída obrigatória: **Mapa do Caos** — seção no PROJECT_STATE.md descrevendo
o que foi encontrado, em linguagem simples.

Por que não tocar em nada ainda: você não sabe o que depende do quê.
Uma mudança "simples" pode quebrar algo que você não sabia que estava conectado.

Gate para R2: Mapa do Caos escrito e revisado com o usuário.

---

### FASE R2 — Triagem
**Objetivo:** classificar os problemas por impacto e risco. Nunca consertar tudo de uma vez.

Classifique cada problema encontrado em:

**P0 — Crítico (conserta primeiro):**
Está quebrando o usuário hoje. Dados sendo perdidos. Segurança comprometida
(senhas no código, banco exposto). Funcionalidade principal não funciona.

**P1 — Urgente (conserta em seguida):**
Vai quebrar em breve. Dívida técnica que impede crescimento.
Ausência de backup. Sem controle de versão.

**P2 — Importante (planeja para depois):**
Código confuso mas funcional. Estrutura de pastas desorganizada.
Falta de documentação. Ausência de testes.

**P3 — Melhorias (backlog):**
Otimizações, refatorações cosméticas, novas funcionalidades.

Regra: nunca misturar P0 com P3 na mesma sessão. Foco sequencial.

Saída obrigatória: **Lista de Triagem** no PROJECT_STATE.md com todos os
problemas classificados.

Gate para R2: Lista de Triagem aprovada pelo usuário.

---

### FASE R3 — Estabilização
**Objetivo:** colocar o projeto num estado seguro para trabalhar.

O que fazer ANTES de qualquer refatoração:
- Inicializar git se não existir e fazer commit do estado atual
  ("snapshot do caos" — importante ter este ponto de retorno)
- Documentar variáveis de ambiente necessárias em `.env.example`
- Mover segredos (senhas, chaves) para `.env` se estiverem no código
  (P0 de segurança — corrigir imediatamente)
- Verificar e documentar dependências (o que precisa estar instalado)
- Criar README mínimo: o que é, como instalar, como rodar

Por que estabilizar antes de refatorar: git é o seguro de vida da Trilha
Vermelha. Sem ele, cada mudança é irreversível.

Gate para R4: commit feito, README existe, segredos fora do código.

---

### FASE R4 — Remediação
**Objetivo:** corrigir P0s e P1s na ordem certa, um por vez.

Protocolo obrigatório para cada correção:
1. **Defina o critério de sucesso ANTES:** "está corrigido quando X funciona"
2. **Faça commit do estado atual** antes de começar a correção
3. **Uma correção por vez** — nunca refatorar arquivo A enquanto corrige B
4. **Execute `/vd-check`** após cada correção antes de partir para a próxima
5. **Se o check falhar:** reverta para o commit anterior, analise, tente novamente

Ordem de correção recomendada:
1. Segurança (senhas/chaves expostas)
2. Perda de dados (backups, transações)
3. Funcionalidade principal quebrada
4. Estrutura de código (imports quebrados, dependências faltando)
5. Organização e documentação

P2s e P3s entram no backlog — não nesta fase.

Gate para R5: todos os P0s resolvidos e verificados, P1s resolvidos ou
com plano documentado.

---

### FASE R5 — Documentação e Graduação
**Objetivo:** documentar o estado atual e promover o projeto para a Trilha Verde.

O que fazer:
- Escrever README completo: o que é, como instalar, variáveis de ambiente,
  como rodar, estrutura de pastas explicada
- Documentar as decisões de arquitetura encontradas (mesmo que não ideais)
  no Decision Log — Chesterton: entenda por que existe antes de remover
- Atualizar PROJECT_STATE.md com o estado real atual
- Registrar post-mortem da Trilha Vermelha: o que foi encontrado, o que
  foi corrigido, o que ficou como dívida técnica conhecida

**Graduação:** ao concluir R5, o projeto entra na Trilha Verde na fase
correspondente ao seu estado atual:
- Sem arquitetura clara → entra na Fase 3 (Arquitetura)
- Arquitetura ok, sem versionamento → entra na Fase 5 (Versionamento)
- Base técnica ok, construindo features → entra na Fase 6 (Construção)

O usuário decide em qual fase verde entrar, com recomendação documentada.

---

## Aviso importante para a Trilha Vermelha

Projetos em produção têm usuários reais. Qualquer mudança que afete
funcionalidade em uso deve ser:
1. Planejada com `/vd-plan` mostrando o impacto
2. Aprovada explicitamente pelo usuário
3. Feita com commit de backup imediatamente antes
4. Verificada com `/vd-check` antes de considerar concluída

"Funciona na minha máquina" não é evidência. O usuário executou e confirmou.
