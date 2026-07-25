# Nível 3 — Micro (Saúde & Débito Técnico)

> Qualidade de código granular. Resposta: "onde está o problema, qual a
> gravidade, quanto custa arrumar". Foco nos 3 buracos que esta skill
> nasceu pra fechar (camadas, anti-patterns, dep vs custom) + cobertura de testes.

## Comando

`/vd-arch-health` (só nível 3) ou parte do `/vd-arch-full`.

## Categorias de varredura

### 1. Separação de camadas (UI / Domínio / Infra)

**O que procurar:**
- UI importando driver de banco (pg, mysql2, prisma direto)
- Controller com SQL inline
- View com regra de negócio
- Lógica de negócio em arquivo de rota
- Model de domínio vazando pro ORM

```bash
# Heurística: UI importando infra
grep -rn "from 'prisma\|from 'pg\|from 'mysql\|import.*prisma\|import.*pg" \
  src/app/ src/components/ 2>/dev/null | head -10

# Controllers com SQL
grep -rn "SELECT\|INSERT\|UPDATE\|DELETE" src/controllers/ 2>/dev/null | head -10

# Models de domínio misturados com ORM
grep -rn "prisma\." src/domain/ src/core/ 2>/dev/null | head -10
```

**Achados típicos:**
- 🔴 `UserController` importando `prisma` direto (camada UI conhecendo schema)
- 🔴 SQL inline em `routes/checkout.ts` (vazamento de abstração)
- 🟡 View com cálculo de imposto (lógica de domínio em camada de apresentação)
- 🟡 Model de domínio + ORM no mesmo arquivo (mistura)

### 2. Anti-patterns de código

**Arquivos genéricos suspeitos:**
```bash
# Procurar nomes vagos
find src -type f \( -name "util*" -o -name "helper*" -o -name "common*" \
  -o -name "misc*" -o -name "manager*" -o -name "shared*" \) 2>/dev/null

# Tamanho dos util.ts (suspeito se > 200 linhas)
find src -name "util*.ts" -o -name "helper*.ts" 2>/dev/null \
  | xargs wc -l 2>/dev/null | sort -rn | head -5
```

**God objects / fat controllers:**
```bash
# Top 10 maiores arquivos
find src -name "*.ts" -not -path "*/node_modules/*" \
  | xargs wc -l 2>/dev/null | sort -rn | head -11

# Procurar por "Manager" / "Controller" / "Service" gordos
find src -name "*Manager*" -o -name "*Controller*" -o -name "*Service*" 2>/dev/null \
  | xargs wc -l 2>/dev/null | sort -rn | head -10
```

**Hotspots (arquivos mais modificados):**
```bash
# Top 10 arquivos mais mexidos
git log --pretty=format: --name-only | grep -v "^$" \
  | sort | uniq -c | sort -rn | head -10
```

**Achados típicos:**
- 🔴 `utils/index.ts` com 1500 linhas
- 🔴 `UserManager.ts` com 80 métodos
- 🔴 Hotspot `src/db/queries.ts` com 5000 mudanças no histórico
- 🟡 `helpers.ts` com 200 linhas
- 🔵 `common.ts` poderia ser dividido

### 3. Acoplamento

**Imports cíclicos:**
```bash
# Com madge (se instalado)
npx madge --circular src/ 2>/dev/null

# Heurística: arquivos com mesmo nome se importando
grep -rn "^import.*from.*'\./" src/ | head -30
```

**Fan-out excessivo (arquivo que importa muitos outros):**
```bash
# Quem tem mais imports (suspeito > 20)
for f in $(find src -name "*.ts" -not -path "*/node_modules/*" | head -50); do
  count=$(grep -c "^import" "$f" 2>/dev/null || echo 0)
  if [ "$count" -gt 15 ]; then
    echo "$count $f"
  fi
done | sort -rn | head -10
```

**Achados típicos:**
- 🔴 `src/index.ts` importando 80+ módulos
- 🔴 Ciclo `UserService` ↔ `AccountService`
- 🟡 `utils/index.ts` importado em 40+ lugares (alto fan-in, mas suspeito)

### 4. Vazamento de abstração

**Sinais:**
- View sabendo schema de banco
- Controller executando regra de domínio
- Lógica de cálculo em arquivo de rota
- DTO vazando entidade
- ORM sendo usado fora da camada de dados

```bash
# UI / componentes usando Prisma
grep -rn "import.*prisma" src/components/ src/app/ 2>/dev/null

# Regras de negócio em controllers
grep -rn "if.*discount\|if.*tax\|if.*price.*>" src/controllers/ 2>/dev/null

# Cálculos em rotas
grep -rn "calculate\|compute\|total.*=" src/routes/ 2>/dev/null
```

**Achados típicos:**
- 🔴 `LoginPage.tsx` importando `prisma`
- 🔴 `checkout/route.ts` com cálculo de desconto inline
- 🟡 `OrderDto` com método `.calculateTotal()` (DTO com regra de negócio)

### 5. Dep externa vs código customizado

**Sinais de dep subutilizada:**
```bash
# Top 5 deps mais importadas
grep -rn "^import" src/ 2>/dev/null \
  | grep -oE "from '(@?[a-z][a-z0-9-]*(/[a-z0-9-]+)*)" \
  | sort | uniq -c | sort -rn | head -10

# Deps no package.json que talvez não sejam usadas
# (heurística simples: nome no package.json mas 0 imports)
for dep in $(cat package.json | jq -r '.dependencies | keys[]' 2>/dev/null); do
  count=$(grep -rln "from '$dep\|from \"$dep\|require('$dep\|require(\"$dep" src/ 2>/dev/null | wc -l)
  if [ "$count" -eq 0 ]; then
    echo "CANDIDATO A REMOÇÃO: $dep"
  fi
done
```

**Sinais de código custom reinventando API padrão:**
- Função de formatação de data, mas `date-fns` ou `dayjs` já no projeto
- Função de debounce, mas `lodash` ou similar já no projeto
- Wrapper de fetch, mas `axios` ou similar já no projeto
- Hash de senha custom (sempre substitua por `bcrypt`/`argon2`)

**Achados típicos:**
- 🔴 Implementação custom de bcrypt (NUNCA faça isso)
- 🟡 `formatDate.ts` com 30 linhas mas `date-fns` já importado
- 🟡 `debounce.ts` mas `lodash` no projeto
- 🔵 `camelCase.ts` mas poderia usar lib padrão

### 6. Cobertura de testes

```bash
# Heurística: arquivos sem teste
total_src=$(find src -name "*.ts" -not -name "*.test.ts" -not -name "*.spec.ts" \
  -not -path "*/node_modules/*" | wc -l)
total_tests=$(find src -name "*.test.ts" -o -name "*.spec.ts" | wc -l)
echo "Ratio: $total_tests / $total_src"

# Hotspots sem teste
for f in $(find src -name "*.ts" -not -name "*.test.ts" -not -name "*.spec.ts" \
  -not -path "*/node_modules/*"); do
  base=$(basename "$f" .ts)
  has_test=$([ -f "${f%.ts}.test.ts" ] || [ -f "${f%.ts}.spec.ts" ] \
    || [ -f "$(dirname $f)/__tests__/${base}.test.ts" ] && echo "yes" || echo "no")
  if [ "$has_test" = "no" ]; then
    # Verificar se é crítico (path de UI / db / payment)
    if echo "$f" | grep -qE "(db|payment|auth|api|routes)"; then
      echo "CRÍTICO SEM TESTE: $f"
    fi
  fi
done | head -10
```

**Achados típicos:**
- 🔴 `src/db/payment.ts` sem teste (caminho crítico)
- 🟡 `src/auth/login.ts` sem teste
- 🟡 80% dos arquivos sem teste (projeto sem cultura de teste)
- 🔵 `src/utils/formatDate.ts` sem teste (código puro, baixo risco)

### 7. Documentação

```bash
# Arquivos com docstrings / JSDoc
grep -rln "^/\*\*" src/ 2>/dev/null | wc -l
echo "---"
find src -name "*.ts" -not -path "*/node_modules/*" | wc -l

# README?
ls README.md docs/ ARCH_MAP.md 2>/dev/null
```

**Achados típicos:**
- 🔴 Sem README, sem ARCH_MAP.md, sem doc
- 🟡 README existe mas desatualizado
- 🟡 Zero JSDoc em módulo público
- 🔵 Falta ARCH_MAP.md (essa skill ajuda a gerar)

## Formato de saída (consolidado)

```markdown
## Micro — Saúde & Débito Técnico

### Resumo executivo
- Total de achados: 13
- Por severidade: 🔴 2 / 🟡 6 / 🔵 5
- Por categoria: Camadas 1 / Anti-pattern 3 / Acoplamento 2 / Abstração 1 / Dep vs Custom 2 / Teste 3 / Docs 1

### Lista priorizada (top 5)

#### 1. [🔴] UserController com SQL inline
- Categoria: Camadas
- Local: src/controllers/UserController.ts:42-78
- Esforço: 🟡 1h
- Sugestão: extrair UserRepository

#### 2. [🔴] Hotspot db/queries.ts sem teste
- Categoria: Teste
- Local: src/db/queries.ts (5000 mudanças históricas)
- Esforço: 🔴 2h+
- Sugestão: priorizar teste de integração

[outros...]

### Veredito: BLOQUEAR (2 bloqueantes, 6 amarelos)
```

## Persistência

Salvar em `.arch-review/micro-health.md` e referenciar no `ARCH_MAP.md`.
