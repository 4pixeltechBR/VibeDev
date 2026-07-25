---
name: vd-arch-review
description: Audita arquitetura de código em 3 níveis (macro/meso/micro). Detecta separação de camadas, anti-patterns, vazamento de abstração, dependências externas vs código customizado, acoplamento. User-invoked, sob demanda, para devs (não leigo). Use quando o usuário pede "auditoria arquitetural", "code review arquitetural", "verifica estrutura do código", "análise de arquitetura", ou após mudanças grandes em estrutura.
disable-model-invocation: true
---

# /vd-arch-review

> **Skill satélite de auditoria arquitetural.** 3ª skill da família VibeDev
> (depois de VibeDev e VibeShield). User-invoked, sob demanda, focada em
> público dev (não leigo).
>
> Inspirado em `improve-codebase-architecture` de mattpocock/skills
> (referência competitiva) e na escada de 5 níveis de verificação de
> Sandeco Macedo (arXiv:2607.00038), adaptada pra VibeDev.

## Tipo de invocação

**User-invoked.** Só humano chama. Modelo nunca dispara sozinho.

Por quê: auditoria arquitetural é decisão humana, não automatizável. Dev
escolhe quando rodar, em qual escopo.

## 3 sub-comandos

| Comando | Níveis | Escopo | Quando usar |
|---|---|---|---|
| `/vd-arch-full` | 1 + 2 + 3 | Stack + Arquitetura + Saúde | Onboarding em código legado, decisão de "vale a pena modernizar" |
| `/vd-arch-only` | 2 | Arquitetura + Componentes | Adicionar módulo novo, ver onde plugar |
| `/vd-arch-health` | 3 | Saúde + Débito Técnico | Antes de PR grande, antes de release, auditar qualidade |

## Os 3 níveis (baseado em Sandeco, adaptado)

### Nível 1 — Macro (Stack/Topologia)

**Escopo:** inventário de superfície.

- Linguagens usadas (TS, Python, Go, etc)
- Frameworks principais (Next.js, Django, etc)
- Banco de dados e ORM
- Sistema de build / package manager
- Contêineres / orquestração
- Dependências externas (diret vs transitivas)
- Estrutura de pastas de alto nível

**Saída:** lista de inventário + diagrama Mermaid de containers.

### Nível 2 — Meso (Arquitetura/Fluxo)

**Escopo:** como o código se organiza.

- Módulos / bounded contexts
- Limites de responsabilidade (quem chama quem)
- Modelo de dados (entidades + relações)
- Rotas de API (REST, GraphQL, RPC)
- Fluxos críticos (login, pagamento, etc) em diagramas de sequência

**Saída:** diagrama de componentes Mermaid + diagrama de sequência dos fluxos principais.

**Vocabulário** (de mattpocock/skills `codebase-design`):
- **Módulo** — qualquer coisa com interface e implementação
- **Seam** — lugar onde você pode mudar comportamento sem editar ali
- **Adapter** — coisa concreta que satisfaz interface numa seam
- **Deep vs Shallow** — leverage na interface (muito comportamento por pouca interface)

### Nível 3 — Micro (Saúde/Débito)

**Escopo:** qualidade de código granular.

- **Separação de camadas** (UI / Domínio / Infra)
- **Anti-patterns** (arquivos genéricos, god objects, controllers gordos)
- **Acoplamento** (imports cíclicos, fan-out excessivo)
- **Vazamento de abstração** (UI importando SQL, lógica de negócio em controller)
- **Dep vs Custom** (lib usada em 1 lugar só = candidato a remoção; código custom que reinventa API padrão)
- **Cobertura de testes** (hotspots sem teste)
- **Complexidade ciclomática** (funções > X linhas)

**Saída:** lista priorizada de débitos + esboço de refactor.

## Como rodar

### Pré-requisitos
- Projeto com código (não projeto greenfield vazio)
- Sem necessidade de executar código (análise estática)

### Fluxo

1. **Confirma escopo:** "Vou rodar o nível X. Confirma?"
2. **Coleta contexto:** lê estrutura de pastas, package files, alguns arquivos-chave
3. **Análise:** percorre categorias do nível
4. **Compila achados:** lista unificada, ordenada por severidade
5. **Persiste em arquivo:** atualiza `ARCH_MAP.md` na raiz + relatórios em `.arch-review/`
6. **Devolve veredito:** `OK` / `REVISAR` / `BLOQUEAR` (mesmo padrão de VibeShield)

### Formato de achado

```markdown
### Achado [N]
- **Categoria:** Camadas | Acoplamento | Abstração | Dep vs Custom | Convenção | Teste | Documentação | Hotspot
- **Severidade:** 🔴 bloqueante | 🟡 amarelo | 🔵 nit
- **Local:** `path/to/file.ext:linha-linha`
- **Trecho:** `<código de 2-3 linhas>`
- **Problema:** [descrição em 1-2 frases]
- **Sugestão:** [como resolver]
- **Esforço estimado:** 🟢 15min | 🟡 1h | 🔴 2h+
```

## Modo técnico vs leigo

**Padrão:** modo técnico. Esta skill **NÃO** ativa modo leigo. Razão: o público-alvo é dev, não leigo. Glossário técnico é OK.

**Mas:** se o dev chamar `/vd-arch-review` num projeto com `modo_usuario: leigo` no estado, **anuncia antes de rodar** (model-invoked-anunciado, ADR 0006):

> "Detectei que esse projeto está em modo leigo. Você quer rodar a auditoria mesmo assim, ou prefere em modo técnico (cria um projeto separado)?"

## Relação com VibeShield

| VibeShield (segurança) | VibeArchReview (arquitetura) |
|---|---|
| Categorias C1-C8 (auth, secrets, deps) | Categorias: Camadas, Acoplamento, Abstração, etc |
| Foco em vulnerabilidades | Foco em qualidade de código |
| Auto via G1-G7 (sub-tarefa de risco) | Manual via `/vd-arch-review` (decisão humana) |
| Verdict: OK/REVISAR/BLOQUEAR | Verdict: OK/REVISAR/BLOQUEAR (mesmo padrão) |

**Complementares, não substitutos.** Uma arquitetura pode ser segura (VibeShield OK) mas com débitos graves (VibeArchReview BLOQUEAR), ou vice-versa.

## Relação com `vibeshield-code-review`

| `vibeshield-code-review` | `vd-arch-review` |
|---|---|
| Eixo: Standards (convenção) + Spec (cumprimento) | Eixo: Arquitetura (camadas, acoplamento, abstração) |
| Review de diff/PR | Review de codebase inteiro ou módulo |
| Sub-tarefa única | Projeto inteiro |

**Complementares.** `vibeshield-code-review` para 1 PR; `vd-arch-review` para o codebase inteiro.

## Estado persistente

```
projeto/
├── ARCH_MAP.md                          (gerado/atualizado, legível)
└── .arch-review/                        (subdir, gerado)
    ├── macro-stack.md                   (nível 1)
    ├── meso-arch.md                     (nível 2)
    ├── micro-health.md                  (nível 3)
    └── SUMMARY.md                       (índice executivo)
```

**Regra:** rodar nível 2 após nível 1 atualiza o `ARCH_MAP.md`, não recria. Idempotente.

## O que NÃO faz

- Não refatora código (só aponta o que refatorar)
- Não decide se vale modernizar (dev decide)
- Não roda testes / build (análise estática)
- Não substitui architect humano
- Não detecta race conditions em runtime
- Não faz pentest

Ver `.out-of-scope/not-an-arch-audit-tool.md` (criado em v3.7.0).

## Exemplo de uso

```
Dev: /vd-arch-health

IA: "Vou rodar auditoria de saúde (nível 3) no projeto. Confirma?"

Dev: "sim"

IA: [lê estrutura, identifica hotspots via git log, percorre 7 categorias,
    compila achados, atualiza ARCH_MAP.md + .arch-review/micro-health.md]

    ### Achado 1
    - Categoria: Camadas
    - Severidade: 🔴 bloqueante
    - Local: src/controllers/UserController.ts:42-78
    - Trecho: `const user = await db.query("SELECT * FROM users WHERE id = $1", [id])`
    - Problema: Controller importa `db` direto e executa SQL. Camada UI
      conhecendo schema de banco (vazamento de abstração).
    - Sugestão: extrair `UserRepository` (camada de dados), controller chama
      `userRepo.findById(id)`.
    - Esforço: 🟡 1h

    ### Achado 2
    - Categoria: Dep vs Custom
    - Severidade: 🟡 amarelo
    - Local: src/utils/formatDate.ts (3 importadores)
    - Problema: 30 linhas de formatação de data custom. `date-fns` já
      está no projeto e tem `format()` equivalente.
    - Sugestão: remover custom, usar `date-fns/format`. Reduz bundle
      mental (~30 linhas) sem perder funcionalidade.
    - Esforço: 🟢 15min

    [outros achados...]

    Veredito: BLOQUEAR (1 bloqueante, 4 amarelos, 8 nits)

    Próxima ação: priorizar Achado 1 (camadas). Sugestão: criar
    `UserRepository` em `src/repositories/`, atualizar controller.
    Após fix, re-rodar `/vd-arch-health` para confirmar.
```

## Versionamento

| Versão | Data | Mudança |
|---|---|---|
| 1.0.0 | 2026-07-25 | Release inicial. 3 sub-comandos, 3 níveis (macro/meso/micro), user-invoked, persistente em `ARCH_MAP.md` + `.arch-review/`. |
