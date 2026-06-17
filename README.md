# 🚀 VibeDev — Governança para Vibe Coders

[![VibeDev License](https://img.shields.io/github/license/4pixeltechBR/VibeDev?style=flat-square&color=blue)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/4pixeltechBR/VibeDev?style=flat-square)](https://github.com/4pixeltechBR/VibeDev/stargazers)
[![Framework Version](https://img.shields.io/badge/version-2.0.0-emerald?style=flat-square)](SKILL.md)

O **VibeDev** é um framework de governança leve e rigoroso desenhado especificamente para desenvolvimento de software mediado por Inteligência Artificial (AI Code Generation / Vibe Coding) sem a necessidade de background formal em engenharia de software. 

Seu principal objetivo é **proteger o desenvolvedor de decisões técnicas ruins, evitar loops infinitos de depuração (anti-loop) e manter o progresso do projeto mapeado de forma persistente**.

---

## 🧭 Princípio Central: O Estado em Arquivo

No **VibeDev**, a conversa com o chat da IA é volátil, mas o estado do software é sagrado e persistente.
* A fonte única de verdade do projeto é o arquivo `PROJECT_STATE.md` localizado na raiz do diretório.
* **A IA deve ler este arquivo no início de toda sessão e atualizá-lo rigorosamente antes de encerrar.**
* Se houver divergência entre as instruções da conversa e o arquivo, o arquivo sempre vence.

---

## 🛠️ Instalação e Configuração

Para ativar a governança do VibeDev no seu ambiente preferido no Windows, siga as instruções abaixo:

### 1. Claude Desktop App (Chat, Cowork, Code)
Você pode habilitar as regras do VibeDev de duas formas no Claude Desktop:
* **Para um projeto específico:** Copie o conteúdo de [SKILL.md](file:///SKILL.md) e salve-o com o nome de `.clauderules` na raiz da pasta do seu projeto. O Claude lerá o arquivo nativamente ao abrir o diretório.
* **Configuração Global (AppData):**
  1. Pressione `Win + R`, digite `%APPDATA%\Claude` e aperte Enter.
  2. Abra (ou crie) o arquivo `claude_desktop_config.json`.
  3. Adicione o conteúdo do `SKILL.md` dentro do bloco de instruções de sistema ou configure-o por meio de um MCP local.

### 2. Antigravity 2.0 & Cursor
O motor do Antigravity e do Cursor reconhecem regras diretamente na raiz do workspace.
1. Salve o conteúdo de [SKILL.md](file:///SKILL.md) com o nome de `.antigravityrules` ou `.cursorrules` na raiz da pasta do seu projeto.
2. O agente adotará as diretrizes como regras imutáveis de sistema para aquela sessão de trabalho.

### 3. Codex App Desktop
1. Na raiz do seu projeto, crie um arquivo chamado `.codexrules` e cole o conteúdo de [SKILL.md](file:///SKILL.md).
2. Nas preferências do desenvolvedor dentro do aplicativo Codex Desktop, marque a opção *"Read workspace rules on startup"*.

### 4. Claude Code (CLI)
1. Salve o conteúdo de [SKILL.md](file:///SKILL.md) com o nome de `.clauderules` na raiz da pasta do seu projeto.

---

## 🗺️ As Duas Trilhas de Desenvolvimento

O VibeDev guia o projeto através de duas abordagens distintas dependendo do estado atual da sua base de código:

### 🟢 Trilha Verde (Greenfield — Do Zero)
Ideal para quando você tem apenas a ideia e nenhuma linha de código escrita. O foco é planejar e validar antes de construir para não fazer a coisa errada na ordem errada.

```
[Pasta Vazia] ➔ Iniciar Agente ➔ Entrevista de Setup ➔ Geração do PROJECT_STATE.md
```

1. Crie uma pasta vazia para seu projeto (ex: `C:\Projetos\MeuSaaS`).
2. Adicione as regras da ferramenta (ex: `.cursorrules`, `.clauderules`).
3. Diga ao agente: *"Sexta-feira, inicializar novo projeto greenfield."*
4. O agente fará perguntas para definir as bases do arquivo `PROJECT_STATE.md` (veja o template [assets/PROJECT_STATE-green.md](file:///assets/PROJECT_STATE-green.md)).
5. O desenvolvimento seguirá por **8 Fases Sequenciais**, com transições validadas por critérios rígidos em [references/gates.md](file:///references/gates.md).

---

### 🔴 Trilha Vermelha (Rescue / Retrofit — Código Existente)
Ideal para domar projetos que foram desenvolvidos de forma desorganizada ("vibe coding selvagem"), cheios de bugs, sem commits consistentes e sem arquitetura definida.

```
[Pasta com Código] ➔ Iniciar Agente ➔ Varredura Automática ➔ Inventário de Dívidas ➔ Geração do PROJECT_STATE.md
```

1. Abra a ferramenta de IA no diretório do seu código existente.
2. Diga ao agente: *"Sexta-feira, ativar protocolo de resgate técnico (Retrofit)."*
3. O agente varrerá a pasta (ignorando pastas pesadas como `node_modules`) para descobrir a stack real e as principais dependências.
4. O agente gerará um **Mapa do Caos** e uma **Lista de Triagem** (P0 a P3) no `PROJECT_STATE.md` (veja o template [assets/PROJECT_STATE-red.md](file:///assets/PROJECT_STATE-red.md)).
5. Você focará em estabilizar o projeto e remediar os problemas críticos antes de adicionar qualquer nova funcionalidade.

---

## ⌨️ Comandos VibeDev

Ao interagir com o agente regulado pelo VibeDev, utilize os seguintes comandos no chat para controlar o fluxo da sessão:

* `/vd-start` — Diagnóstico inicial para configurar a trilha e o estado inicial.
* `/vd-status` — Exibe um resumo de exatamente 4 linhas (Trilha, Fase Atual, Última Decisão, Próximo Passo).
* `/vd-plan [intenção]` — Cria um plano detalhado de sub-tarefas com estimativa de esforço (🟢, 🟡, 🔴) e critérios de aceitação. **PARE e aguarda sua aprovação** antes de codificar.
* `/vd-build` — Executa de forma ordenada as sub-tarefas aprovadas, exigindo logs e testes de happy-path.
* `/vd-check` — Valida a execução comparando o resultado obtido com o critério definido antes do código.
* `/vd-close` — Encerra formalmente a sessão atualizando o estado, preenchendo o micro-post-mortem e salvando métricas do framework.

---

## ⚠️ Boas Práticas e Ambiente Windows

* **Diretórios de Trabalho:** Evite executar projetos em pastas protegidas do sistema (como `C:\Program Files`). Dê preferência a caminhos no diretório do usuário (ex: `C:\Users\SeuUsuario\Projetos`) para evitar problemas com permissões de gravação de arquivos de estado.
* **Exit Codes Automatizados:** Ao rodar comandos de testes no Windows (como `pytest` ou `npm run test`), o agente inspecionará o `$LastExitCode` no PowerShell. Se houver falha, ele travará o avanço até que o erro seja remediado.
* **Decisões Tipo 1 vs Tipo 2:**
  - **Tipo 1 (Difíceis de reverter):** Escolha de stack, banco de dados, autenticação, estrutura de preços. O agente apresentará 3 opções com prós, contras e um *Red Team* robusto para aprovação.
  - **Tipo 2 (Fáceis de reverter):** Decisões cosméticas e lógica pontual. O agente recomenda uma e executa diretamente para manter a velocidade de desenvolvimento.

---

## 📦 Estrutura do Repositório

* [SKILL.md](file:///SKILL.md) — As regras e prompts fundamentais do framework VibeDev.
* [assets/](file:///assets/) — Contém os templates markdown do `PROJECT_STATE.md` para as trilhas Verde e Vermelha.
* [references/](file:///references/) — Guias técnicos de transição de fase, recomendações de stacks e checklists.

---
Desenvolvido com ❤️ para a comunidade de Vibe Coders pela equipe **4pixeltech**.
