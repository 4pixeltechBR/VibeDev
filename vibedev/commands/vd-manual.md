# /vd-manual

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Sub-comando do `/vd-close`. Gera um `MANUAL.md` automático baseado no
> estado do projeto + arquivos entregues, antes do encerramento formal.

## 🇧🇷 O que faz

Rodado automaticamente dentro do `/vd-close` (opt-out via flag `--no-manual`).

Lê o estado completo do projeto e gera `MANUAL.md` com 5 seções obrigatórias:

1. **Visão geral** — 1 parágrafo: o que é, pra quem, status
2. **Como rodar** — comando(s) pra rodar local
3. **Como usar** — fluxos do usuário, screenshots opcionais
4. **Como estender** — onde mexer pra adicionar feature X
5. **Troubleshooting** — 3+ problemas comuns + solução

Se o projeto está em modo leigo, o manual é em linguagem simples (sem jargão).
Se está em modo técnico, o manual inclui referências a arquivos, classes, funções.

## 🇺🇸 What it does

Auto-run inside `/vd-close` (opt-out via `--no-manual` flag).

Reads full project state and generates `MANUAL.md` with 5 mandatory sections:

1. **Overview** — 1 paragraph: what it is, who for, status
2. **How to run** — command(s) to run locally
3. **How to use** — user flows, optional screenshots
4. **How to extend** — where to touch to add feature X
5. **Troubleshooting** — 3+ common problems + solution

If layman mode, manual is in plain language (no jargon). If technical mode, manual includes references to files, classes, functions.

---

## 📋 Template gerado

```markdown
# MANUAL — [Nome do Projeto]

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Gerado automaticamente por `/vd-close` em AAAA-MM-DD.
> Atualizado por `/vd-close` em cada sessão de encerramento.

## 🇧🇷 Visão geral
[1 parágrafo: o que é, pra quem serve, status atual do projeto]

## 🇺🇸 Overview
[1 paragraph: what it is, who it's for, current project status]

---

## 🇧🇷 Como rodar

[comandos passo-a-passo pra rodar do zero]

## 🇺🇸 How to run

[step-by-step commands to run from scratch]

---

## 🇧🇷 Como usar

[fluxos do usuário, com prints se disponível]

## 🇺🇸 How to use

[user flows, with screenshots if available]

---

## 🇧🇷 Como estender

[onde mexer pra adicionar feature X — paths absolutos]

## 🇺🇸 How to extend

[where to touch to add feature X — absolute paths]

---

## 🇧🇷 Troubleshooting

[3+ problemas comuns + solução]

## 🇺🇸 Troubleshooting

[3+ common problems + solution]

---

## 📚 Ver também / See also

- [README.md](../README.md)
- [PROJECT_STATE.md](../PROJECT_STATE.md)
- [CHANGELOG.md](../CHANGELOG.md)
- [ARCH_MAP.md](../ARCH_MAP.md) (se existir)
```

---

## 🛠️ Como o agente extrai cada seção

| Seção | Fonte |
|-------|-------|
| Visão geral | Bloco "Contexto para novo agente/sessão" do PROJECT_STATE |
| Como rodar | Stack do PROJECT_STATE + README/INSTALL se existir + arquivos `package.json`, `Makefile`, `docker-compose.yml`, etc |
| Como usar | Comandos, rotas ou endpoints inferidos do código + casos de uso do PROJECT_STATE |
| Como estender | Arquitetura inferida (mapeada a partir de `ARCH_MAP.md` se existir, ou da estrutura de pastas) |
| Troubleshooting | Histórico de Decision Log (problemas resolvidos) + Issues abertos se houver integração com GitHub |

---

## 🚦 Comportamento

| Situação | Ação |
|----------|------|
| Projeto novo (1ª sessão) | Gera manual mínimo (3 seções obrigatórias) |
| Projeto maduro (3+ sessões) | Gera manual completo (5 seções) |
| Modo leigo ativo | Linguagem simples, sem paths absolutos, sem jargão técnico |
| Modo técnico | Paths absolutos, classes, funções, diagramas |
| Sem `PROJECT_STATE.md` | Recusa rodar, pede pra rodar `/vd-start` primeiro |
| `MANUAL.md` já existe | Atualiza (preserva seções customizadas) |

---

## 🧪 Exemplo (modo técnico)

```markdown
# MANUAL — TaskFlow API

> 🇧🇷 PT-BR · 🇺🇸 EN follows
>
> Gerado automaticamente por `/vd-close` em 2026-07-26.
> Última atualização: 2026-07-26.

## 🇺🇸 Overview

TaskFlow is a REST API for personal task management, written in FastAPI + PostgreSQL.
It serves 12 active users and processes ~500 tasks/day. Status: production MVP
(Phase 7 — launched 2026-07-15).

---

## 🇺🇸 How to run

\`\`\`bash
# 1. Clone
git clone https://github.com/4pixeltech/taskflow.git
cd taskflow

# 2. Install
poetry install

# 3. Database
docker compose up -d db
poetry run alembic upgrade head

# 4. Run
poetry run uvicorn taskflow.main:app --reload

# 5. Open
open http://localhost:8000/docs
\`\`\`

---

## 🇺🇸 How to use

### Create a task
\`\`\`bash
curl -X POST http://localhost:8000/tasks \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"title": "Buy milk", "due": "2026-07-27"}'
\`\`\`

### List my tasks
\`\`\`bash
curl http://localhost:8000/tasks/me \
  -H "Authorization: Bearer $TOKEN"
\`\`\`

---

## 🇺🇸 How to extend

To add a new endpoint:
- Add the route in `taskflow/api/routes/tasks.py`
- Add the service in `taskflow/services/task_service.py`
- Add the schema in `taskflow/schemas/task.py`
- Add the test in `tests/test_tasks.py`
- Run `poetry run pytest` to verify

---

## 🇺🇸 Troubleshooting

### "Connection refused" on database
Postgres isn't running. Run `docker compose up -d db`.

### "Invalid token" on every request
Token expired (24h TTL). POST `/auth/refresh` with old token.

### "Module not found" after pull
Dependencies changed. Run `poetry install`.
```

---

## 🔗 Ver também

- `/vd-close` — comando que invoca este sub-comando
- `/vd-launch` — material de lançamento (parente próximo)
- `references/handoffs.md` — como contexto passa entre sessões
- `MANUAL.md` (raiz) — manual geral do VibeDev (este aqui é o de projetos)

---

## 📚 Inspirações

- **Dante Testa — Prompt Mestre WordPress v1.14.0** (jul/2026): "acompanhado de um manual de uso ilustrado"
- **Cargo Book** (Rust): "rustup doc" gera docs locais automaticamente
- **Godot Documentation Generator**: gera manual de jogo a partir de comentários
- **Conventional Comments** (git): padroniza formato de comunicação
