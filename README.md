# 🚀 VibeDev — Governança para Vibe Coders / Governance for Vibe Coders

🌍 **Read in / Leia em:** [English](#-english) | [Português](#-português)

---

## 🇺🇸 English

[![VibeDev License](https://img.shields.io/github/license/4pixeltechBR/VibeDev?style=flat-square&color=blue)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/4pixeltechBR/VibeDev?style=flat-square)](https://github.com/4pixeltechBR/VibeDev/stargazers)
[![Framework Version](https://img.shields.io/badge/version-2.1.0-emerald?style=flat-square)](SKILL.md)

**VibeDev** is a lightweight, rigorous governance framework specifically designed for AI-driven software development (AI Code Generation / Vibe Coding) without requiring a formal background in software engineering.

Its main goal is to **protect the developer from poor technical choices, prevent infinite debugging loops (anti-loop), and keep project progress persistently mapped**.

---

### 🧭 Core Principle: State in File

In **VibeDev**, the AI chat session is volatile, but the software state is sacred and persistent.
* The single source of truth for the project is the `PROJECT_STATE.md` file located at the root of the directory.
* **The AI must read this file at the start of every session and update it rigorously before closing.**
* If there is any discrepancy between the conversation instructions and the file, the file always wins.

---

### 🛠️ Installation and Configuration

To enable VibeDev governance in your preferred environment on Windows, follow the instructions below:

#### 1. Claude Desktop App (Chat, Cowork, Code)
You can enable VibeDev rules in Claude Desktop in two ways:
* **For a specific project:** Copy the content of [SKILL.md](file:///SKILL.md) and save it as `.clauderules` at the root of your project folder. Claude will read the file natively when opening the directory.
* **Global Configuration (AppData):**
  1. Press `Win + R`, type `%APPDATA%\Claude` and hit Enter.
  2. Open (or create) the `claude_desktop_config.json` file.
  3. Add the content of `SKILL.md` inside the system prompt configuration block or set it up via a local MCP server.

#### 2. Antigravity 2.0 & Cursor
The Antigravity and Cursor engines recognize rules directly at the root of the workspace.
1. Save the content of [SKILL.md](file:///SKILL.md) as `.antigravityrules` or `.cursorrules` at the root of your project folder.
2. The agent will adopt the guidelines as immutable system rules for that working session.

#### 3. Codex App Desktop
1. At the root of your project, create a file named `.codexrules` and paste the content of [SKILL.md](file:///SKILL.md).
2. In the developer preferences inside the Codex Desktop app, check the *"Read workspace rules on startup"* option.

#### 4. Claude Code (CLI)
1. Save the content of [SKILL.md](file:///SKILL.md) as `.clauderules` at the root of your project folder.

---

### 🗺️ The Two Development Tracks

VibeDev guides the project through two distinct approaches depending on the current state of your codebase:

#### 🟢 Green Track (Greenfield — From Scratch)
Ideal when you only have the idea and zero lines of code written. The focus is on planning and validating before building to avoid doing the wrong thing in the wrong order.

```
[Empty Folder] ➔ Start Agent ➔ Setup Interview ➔ Geração do PROJECT_STATE.md
```

1. Create an empty folder for your project (e.g., `C:\Projects\MySaaS`).
2. Add the tool rules file (e.g., `.cursorrules`, `.clauderules`).
3. Tell the agent: *"Sexta-feira, inicializar novo projeto greenfield."*
4. The agent will ask questions to define the bases of the `PROJECT_STATE.md` file (see template [assets/PROJECT_STATE-green.md](file:///assets/PROJECT_STATE-green.md)).
5. Development will proceed through **8 Sequential Phases**, with transitions validated by strict criteria in [references/gates.md](file:///references/gates.md).

#### 🔴 Red Track (Rescue / Retrofit — Existing Code)
Ideal for taming projects developed in a disorganized manner ("wild vibe coding"), full of bugs, missing consistent commits, and lacking a defined architecture.

```
[Folder with Code] ➔ Start Agent ➔ Auto-Scan ➔ Debt Inventory ➔ Geração do PROJECT_STATE.md
```

1. Open your AI tool in your existing code directory.
2. Tell the agent: *"Sexta-feira, ativar protocolo de resgate técnico (Retrofit)."*
3. The agent will scan the folder (ignoring heavy folders like `node_modules`) to discover the actual stack and key dependencies.
4. The agent will generate a **Chaos Map** and a **Triage List** (P0 to P3) in `PROJECT_STATE.md` (see template [assets/PROJECT_STATE-red.md](file:///assets/PROJECT_STATE-red.md)).
5. You will focus on stabilizing the project and remediating critical issues before adding any new features.

---

### ⌨️ VibeDev Commands

When interacting with an agent regulated by VibeDev, use the following commands in the chat to control the session flow:

* `/vd-start` — Initial diagnosis to set up the track and initial state.
* `/vd-status` — Displays a quick 4-line summary (Track, Current Phase, Last Decision, Next Step).
* `/vd-plan [intent]` — Creates a detailed plan of sub-tasks with effort estimates (🟢, 🟡, 🔴) and acceptance criteria. **PAUSES and waits for your approval** before coding.
* `/vd-build` — Executes the approved sub-tasks in an orderly fashion, requiring logs and happy-path tests.
* `/vd-check` — Validates execution by comparing the actual outcome with the criteria defined before coding.
* `/vd-close` — Formally closes the session by updating the state, filling the micro-post-mortem, and saving framework metrics.

---

### ⚠️ Best Practices and Windows Environment

* **Working Directories:** Avoid running projects in protected system folders (like `C:\Program Files`). Prefer paths in the user directory (e.g., `C:\Users\YourUser\Projects`) to prevent write permission issues with state files.
* **Automated Exit Codes:** When running test commands on Windows (like `pytest` or `npm run test`), the agent will inspect `$LastExitCode` in PowerShell. If a test fails, it will lock progress until the error is remediated.
* **Type 1 vs Type 2 Decisions:**
  - **Type 1 (Hard to reverse):** Choosing a stack, database, authentication, or pricing structure. The agent will present 3 options with pros, cons, and a robust *Red Team* analysis for approval.
  - **Type 2 (Easy to reverse):** Cosmetic decisions and localized logic. The agent recommends one and executes it directly to maintain development speed.

---

### 📦 Repository Structure

* [SKILL.md](file:///SKILL.md) — The fundamental rules and prompts of the VibeDev framework.
* [assets/](file:///assets/) — Contains the `PROJECT_STATE.md` markdown templates for both Green and Red tracks.
* [references/](file:///references/) — Technical guides on phase transitions, stack recommendations, and checklists.

---

## 🇧🇷 Português

[![VibeDev License](https://img.shields.io/github/license/4pixeltechBR/VibeDev?style=flat-square&color=blue)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/4pixeltechBR/VibeDev?style=flat-square)](https://github.com/4pixeltechBR/VibeDev/stargazers)
[![Framework Version](https://img.shields.io/badge/version-2.0.0-emerald?style=flat-square)](SKILL.md)

O **VibeDev** é um framework de governança leve e rigoroso desenhado especificamente para desenvolvimento de software mediado por Inteligência Artificial (AI Code Generation / Vibe Coding) sem a necessidade de background formal em engenharia de software. 

Seu principal objetivo é **proteger o desenvolvedor de decisões técnicas ruins, evitar loops infinitos de depuração (anti-loop) e manter o progresso do projeto mapeado de forma persistente**.

---

### 🧭 Princípio Central: O Estado em Arquivo

No **VibeDev**, a conversa com o chat da IA é volátil, mas o estado do software é sagrado e persistente.
* A fonte única de verdade do projeto é o arquivo `PROJECT_STATE.md` localizado na raiz do diretório.
* **A IA deve ler este arquivo no início de toda sessão e atualizá-lo rigorosamente antes de encerrar.**
* Se houver divergência entre as instruções da conversa e o arquivo, o arquivo sempre vence.

---

### 🛠️ Instalação e Configuração

Para ativar a governança do VibeDev no seu ambiente preferido no Windows, siga as instruções abaixo:

#### 1. Claude Desktop App (Chat, Cowork, Code)
Você pode habilitar as regras do VibeDev de duas formas no Claude Desktop:
* **Para um projeto específico:** Copie o conteúdo de [SKILL.md](file:///SKILL.md) e salve-o com o nome de `.clauderules` na raiz da pasta do seu projeto. O Claude lerá o arquivo nativamente ao abrir o diretório.
* **Configuração Global (AppData):**
  1. Pressione `Win + R`, digite `%APPDATA%\Claude` e aperte Enter.
  2. Abra (ou crie) o arquivo `claude_desktop_config.json`.
  3. Adicione o conteúdo do `SKILL.md` dentro do bloco de instruções de sistema ou configure-o por meio de um MCP local.

#### 2. Antigravity 2.0 & Cursor
O motor do Antigravity e do Cursor reconhecem regras diretamente na raiz do workspace.
1. Salve o conteúdo de [SKILL.md](file:///SKILL.md) com o nome de `.antigravityrules` ou `.cursorrules` na raiz da pasta do seu projeto.
2. O agente adotará as diretrizes como regras imutáveis de sistema para aquela sessão de trabalho.

#### 3. Codex App Desktop
1. Na raiz do seu projeto, crie um arquivo chamado `.codexrules` e cole o conteúdo de [SKILL.md](file:///SKILL.md).
2. Nas preferências do desenvolvedor dentro do aplicativo Codex Desktop, marque a opção *"Read workspace rules on startup"*.

#### 4. Claude Code (CLI)
1. Salve o conteúdo de [SKILL.md](file:///SKILL.md) com o nome de `.clauderules` na raiz da pasta do seu projeto.

---

### 🗺️ As Duas Trilhas de Desenvolvimento

O VibeDev guia o projeto através de duas abordagens distintas dependendo do estado atual da sua base de código:

#### 🟢 Trilha Verde (Greenfield — Do Zero)
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

#### 🔴 Trilha Vermelha (Rescue / Retrofit — Código Existente)
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

### ⌨️ Comandos VibeDev

Ao interagir com o agente regulado pelo VibeDev, utilize os seguintes comandos no chat para controlar o fluxo da sessão:

* `/vd-start` — Diagnóstico inicial para configurar a trilha e o estado inicial.
* `/vd-status` — Exibe um resumo de exatamente 4 linhas (Trilha, Fase Atual, Última Decisão, Próximo Passo).
* `/vd-plan [intenção]` — Cria um plano detalhado de sub-tarefas com estimativa de esforço (🟢, 🟡, 🔴) e critérios de aceitação. **PARE e aguarda sua aprovação** antes de codificar.
* `/vd-build` — Executa de forma ordenada as sub-tarefas aprovadas, exigindo logs e testes de happy-path.
* `/vd-check` — Valida a execução comparando o resultado obtido com o critério definido antes do código.
* `/vd-close` — Encerra formalmente a sessão atualizando o estado, preenchendo o micro-post-mortem e salvando métricas do framework.

---

### ⚠️ Boas Práticas e Ambiente Windows

* **Diretórios de Trabalho:** Evite executar projetos em pastas protegidas do sistema (como `C:\Program Files`). Dê preferência a caminhos no diretório do usuário (ex: `C:\Users\SeuUsuario\Projetos`) para evitar problemas com permissões de gravação de arquivos de estado.
* **Exit Codes Automatizados:** Ao rodar comandos de testes no Windows (como `pytest` ou `npm run test`), o agente inspecionará o `$LastExitCode` no PowerShell. Se houver falha, ele travará o avanço até que o erro seja remediado.
* **Decisões Tipo 1 vs Tipo 2:**
  - **Tipo 1 (Difíceis de reverter):** Escolha de stack, banco de dados, autenticação, estrutura de preços. O agente apresentará 3 opções com prós, contras e um *Red Team* robusto para aprovação.
  - **Tipo 2 (Fáceis de reverter):** Decisões cosméticas e lógica pontual. O agente recomenda uma e executa diretamente para manter a velocidade de desenvolvimento.

---

### 📦 Estrutura do Repositório

* [SKILL.md](file:///SKILL.md) — As regras e prompts fundamentais do framework VibeDev.
* [assets/](file:///assets/) — Contém os templates markdown do `PROJECT_STATE.md` para as trilhas Verde e Vermelha.
* [references/](file:///references/) — Guias técnicos de transição de fase, recomendações de stacks e checklists.

---
Desenvolvido com ❤️ para a comunidade de Vibe Coders pela equipe **4pixeltech**.
