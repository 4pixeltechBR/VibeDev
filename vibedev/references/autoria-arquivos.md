# Autoria de Arquivos (v3.8+)

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Define como agentes VibeDev devem (ou não) assinar arquivos gerados, baseado
> no bloco `## Autoria` do `PROJECT_STATE.md`.

## 🇧🇷 Como funciona

Se o bloco `## Autoria` do `PROJECT_STATE.md` estiver preenchido com `Incluir header de assinatura nos arquivos: [x] sim`, **todo arquivo novo gerado** pelo agente recebe um header de assinatura no topo.

Se estiver `[ ] não` ou vazio, **nada é adicionado**.

## 🇺🇸 How it works

If the `## Autoria` block in `PROJECT_STATE.md` is filled with `Incluir header de assinatura nos arquivos: [x] sim`, every new file the agent generates gets an authorship header at the top.

If `[ ] não` or empty, **nothing is added**.

---

## 📐 Formato do header (por linguagem)

O agente detecta a linguagem pela extensão do arquivo e gera o comentário apropriado.

### Linguagens com `//` (JS, TS, Go, Rust, Java, C, C++, Swift, Kotlin, PHP)
```js
//
// Autoria: [Nome] <[Email]>
// Projeto: [Projeto] · VibeDev v[versão]
// Gerado em: AAAA-MM-DD
// Licença: [a do projeto, default MIT]
// Veja: PROJECT_STATE.md
//
```

### Linguagens com `#` (Python, Ruby, Shell, YAML, TOML)
```python
#
# Autoria: [Nome] <[Email]>
# Projeto: [Projeto] · VibeDev v[versão]
# Gerado em: AAAA-MM-DD
# Licença: [a do projeto, default MIT]
# Veja: PROJECT_STATE.md
#
```

### Linguagens com `<!-- -->` (HTML, Markdown, XML)
```html
<!--
  Autoria: [Nome] <[Email]>
  Projeto: [Projeto] · VibeDev v[versão]
  Gerado em: AAAA-MM-DD
  Licença: [a do projeto, default MIT]
  Veja: PROJECT_STATE.md
-->
```

### Linguagens com `--` (SQL, Lua, Haskell, Ada)
```sql
--
-- Autoria: [Nome] <[Email]>
-- Projeto: [Projeto] · VibeDev v[versão]
-- Gerado em: AAAA-MM-DD
-- Licença: [a do projeto, default MIT]
-- Veja: PROJECT_STATE.md
--
```

### Linguagens com `(* *)` (Pascal, OCaml)
```pascal
(*
  Autoria: [Nome] <[Email]>
  Projeto: [Projeto] · VibeDev v[versão]
  Gerado em: AAAA-MM-DD
  Licença: [a do projeto, default MIT]
  Veja: PROJECT_STATE.md
*)
```

### Formatos sem comentário (JSON, CSV, binário)
- **Não adicionar header** (quebraria o parser).
- Adicionar arquivo companion `[nome].meta.json` ao lado, com a metadata.

---

## 📋 Comportamento por tipo de arquivo

| Tipo | Comportamento |
|------|---------------|
| Código fonte | Header no topo do arquivo |
| Markdown (`.md`) | Header no topo do arquivo |
| HTML/XML | Header no topo do arquivo |
| JSON | Companion `.meta.json` |
| CSV / binário | Companion `.meta.json` |
| Imagem / asset | Companion `.meta.txt` (1 linha) |
| Migração de DB | Header SQL + `migration_meta` table se aplicável |
| Config (`.env.example`) | NÃO adicionar (risco de vazamento) |

---

## 🔒 LGPD e privacidade

- Se `Email` for vazio, omite `<[Email]>`.
- Se `Organização` for vazio, omite.
- O `Email` NUNCA deve ser um dado sensível (CPF, telefone, etc).
- O agente **não compartilha o `PROJECT_STATE.md` automaticamente** — fica local.

---

## 🛠️ Trilha Vermelha (resgate)

No template `PROJECT_STATE-red.md`, há flag adicional: `Marcar arquivos NOVOS vs legados: [x] sim`. Quando ativo, arquivos novos recebem sufixo `[NOVO]` no header:

```js
// [NOVO] Autoria: [Nome] <[Email]>
```

Arquivos legados (não tocados) permanecem sem header. Arquivos modificados (gerados → editados pelo agente) recebem `[EDITADO]`:

```js
// [EDITADO] Autoria: [Nome] <[Email]>
```

---

## 🔗 Ver também

- [PROJECT_STATE-green.md](../assets/PROJECT_STATE-green.md) — bloco `## Autoria` no template
- [PROJECT_STATE-green-leigo.md](../assets/PROJECT_STATE-green-leigo.md) — versão leigo
- [PROJECT_STATE-red.md](../assets/PROJECT_STATE-red.md) — versão Trilha Vermelha
- [decisao-stack.md](decisao-stack.md) — para o campo `Linguagem/Framework`
- [glossario-leigo.md](glossario-leigo.md) — termos explicados em modo leigo

---

## 📚 Inspirações

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026): "Cada arquivo gerado leva sua assinatura de autoria."
- **LGPD Art. 37** (Brasil): "O tratamento de dados pessoais deve ser precedido de registro das operações de tratamento."
- **EU AI Act Art. 12** (Europa, 2024): "Logs automatizados devem garantir rastreabilidade."
