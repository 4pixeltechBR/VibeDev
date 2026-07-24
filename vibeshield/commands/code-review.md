---
name: vibeshield-code-review
description: Review a code diff or PR against two axes: Standards (does the code follow VibeDev conventions?) and Spec (does it implement what the spec said?). Returns a list of findings with severity, axis, and suggested fix.
disable-model-invocation: true
---

# Code Review — VibeShield

Skill dedicada de revisão de código/PR dentro do ciclo VibeDev. Diferente da auditoria gatilho (G1-G7) que dispara em sub-tarefas de risco: **code-review é chamada pelo humano** quando quer revisar o diff inteiro de uma feature.

Inspirado em [`code-review` de mattpocock/skills](https://github.com/mattpocock/skills) (referência competitiva), mas com adaptação pro contexto VibeDev: a revisão é feita **contra o estado do projeto** (não só contra diff cru), e respeita modo leigo/técnico.

## Tipo de invocação

**User-invoked.** Só humano chama. Modelo nunca dispara sozinho. Decisão humana, não automatizável.

## Os 2 eixos

Toda review gera achados em uma (ou ambas) destas categorias:

### Eixo 1: Standards
"Esse código segue convenções que a VibeDev esperaria?"

- Naming consistente com o resto do projeto?
- Estrutura de pastas alinhada com decisões do `PROJECT_STATE.md`?
- Comentários onde precisa (não onde não precisa)?
- Tratamento de erro uniforme?
- Logs no padrão do projeto?
- Testes no padrão definido?
- Dependências justificadas?

### Eixo 2: Spec
"Esse código faz o que a spec (sub-tarefa ativa) diz que deveria fazer?"

- Critério de "pronto" da sub-tarefa tá atendido?
- Casos listados na spec foram cobertos?
- Casos não-listados (anti-escopo) foram respeitados?
- Decisões Tipo 1 do plano foram seguidas?
- Outputs do código batem com outputs esperados?

## Como rodar

### Inputs necessários
- `PROJECT_STATE.md` (carregado automaticamente pela skill)
- Diff ou PR (pode ser colado, ou apontar pra branch/commit)
- Identificação da sub-tarefa ativa (lê do estado)

### Fluxo

1. **Confirma inputs** com o usuário: "Vou revisar o diff X contra a sub-tarefa Y do estado. Certo?"
2. **Coleta diff**: lê arquivos modificados, prepara contexto
3. **Análise Eixo 1 (Standards)**: percorre convenções do projeto
4. **Análise Eixo 2 (Spec)**: compara com critério de pronto da sub-tarefa
5. **Compila achados**: lista unificada, ordenada por severidade
6. **Devolve veredito**:
   - `OK` (sem achados, ou só nits) — pode commitar
   - `REVISAR` (amarelos) — commita mas anota no backlog
   - `BLOQUEAR` (vermelhos) — não commita, voltar pra `/vd-build`

## Formato de achado

Cada achado segue envelope:

```markdown
### Achado [N]
- **Eixo:** Standards | Spec
- **Severidade:** 🔴 bloqueante | 🟡 amarelo | 🔵 nit
- **Arquivo:** `path/to/file.ext:linha`
- **Trecho:** `<código de 2-3 linhas>`
- **Problema:** [descrição em 1-2 frases]
- **Sugestão de fix:** [como resolver]
```

## Modo técnico vs leigo

### Modo Técnico
- Achados em linguagem técnica direta
- Sugestões com referência exata (arquivo:linha, código, diff)
- Veredito explícito + rationale

### Modo Leigo
- Achados em linguagem simples (glossário VibeDev)
- Sugestões como "tenta fazer X" sem referência técnica
- Veredito explícito + pergunta "isso te ajuda a entender o próximo passo?"

## O que NÃO faz

- Não executa código (não roda testes, não builda)
- Não dispara outros skills automaticamente (cada skill tem seu trigger)
- Não edita nada (só lê e devolve achados)
- Não cria arquivo próprio (audit fica no `PROJECT_STATE.md` no envelope padrão)
- Não substitui VibeShield audit gatilho (G1-G7) — code-review é sob demanda

## Relação com VibeShield audit (G1-G7)

| VibeShield audit (gatilho) | VibeShield code-review (sob demanda) |
|---|---|
| Dispara em G1-G7 (auth, dados, API, etc) | Dispara em `/vibeshield-code-review` |
| Foco em categoria C1-C8 | Foco em Standards + Spec |
| Roda em sub-tarefa de risco | Roda em qualquer sub-tarefa |
| Output: OK/REVISAR/BLOQUEAR | Output: lista de achados + verdict |
| User-anunciado (auto) | User-invoked (manual) |

**Complementares, não substitutos.** Uma sub-tarefa pode passar na audit (G1-G7) e falhar no code-review (Standards + Spec), ou vice-versa.

## Exemplo de uso

```
Usuário: /vibeshield-code-review

IA: "Tô revisando a sub-tarefa 3 de 5 da Fase 6 (tela de login). 
    Diff: 4 arquivos, +127/-34 linhas. Confirma?"

Usuário: "sim"

IA: [lê PROJECT_STATE.md, pega critério de pronto, gera diff mentally, 
    percorre os 2 eixos, devolve lista de achados]

    ### Achado 1
    - Eixo: Spec
    - Severidade: 🔴 bloqueante
    - Arquivo: src/auth/login.ts:42
    - Trecho: `if (user.email == input.email) { ... }`
    - Problema: comparação de email sem normalizar (caixa). 
      "Joao@exemplo.com" e "joao@exemplo.com" seriam contas diferentes.
    - Sugestão: usar `user.email.toLowerCase() === input.email.toLowerCase()`
    
    ### Achado 2
    - Eixo: Standards
    - Severidade: 🟡 amarelo
    - ...
    
    Veredito: BLOQUEAR (1 bloqueante, 2 amarelos)
    
    Próxima ação: voltar pra /vd-build, resolver achado 1, 
    re-rodar review, commit.
```

## Versionamento

| Versão | Data | Mudança |
|---|---|---|
| 1.0.0 | 2026-07-24 | Release inicial. Skill dedicada dentro de VibeShield, 2 eixos (Standards + Spec), user-invoked, complementando audit G1-G7. |
