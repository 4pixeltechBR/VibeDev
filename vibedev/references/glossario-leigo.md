# Glossário Ativo — VibeDev (modo leigo)

> Mapeamento termo técnico → linguagem humana.
> Carregado automaticamente quando `modo_usuario: leigo` no PROJECT_STATE.md.
> Aplica em TODA saída: status, plano, gate, check, conversa.
> Sem mudar o significado — só a forma.

---

## Como usar (instrução pra IA)

1. Ao iniciar sessão, leia `modo_usuario` no `PROJECT_STATE.md`.
2. Se for `leigo`, este arquivo é **obrigatório** em TODA resposta.
3. Ao escrever qualquer parágrafo técnico, passe mentalmente pela tabela abaixo.
4. Se aparecer termo mapeado, use a versão humana.
5. Se aparecer termo novo (não listado aqui), **traduza inline na hora** e adicione
   à tabela depois — sem perguntar, é trabalho seu.
6. Mantenha nomes próprios de tecnologia (Python, Postgres, Vercel, Stripe) como
   estão — o problema não é o nome, é o conceito por trás.

---

## Mapeamento termo → humano

### Construção & código

| Termo técnico | Tradução leiga |
|---|---|
| endpoint / rota | o caminho que o sistema responde (ex: `/login`) |
| request / requisição | um pedido que alguém fez pro sistema |
| response / resposta | o que o sistema devolveu pro pedido |
| método HTTP (GET/POST/PUT/DELETE) | tipo de pedido: "me dá", "cria isso", "atualiza isso", "apaga isso" |
| schema | o desenho de como os dados são guardados |
| migration | mudança na estrutura dos dados guardados |
| seed | dados de exemplo pra testar |
| refactor / refatorar | reorganizar o código sem mudar o que ele faz |
| feature | uma funcionalidade, um pedaço do produto |
| bug | defeito, coisa que tá quebrada |
| debug | investigar por que tá quebrado |
| log | registro do que aconteceu (tipo diário do sistema) |
| commit | salvar uma versão do código com mensagem explicando |
| branch | uma cópia paralela pra mexer sem estragar o principal |
| merge | juntar duas cópias de volta numa |
| deploy / publicar | colocar no ar pra pessoas de fora acessarem |
| build | o processo de empacotar o código pra rodar |
| ambiente dev / prod | onde você testa / onde os outros usam |
| dev server | o sistema rodando local na sua máquina pra você mexer |
| hot reload | o sistema atualiza sozinho enquanto você salva |

### Banco de dados

| Termo técnico | Tradução leiga |
|---|---|
| banco de dados | onde os dados ficam guardados |
| tabela | uma lista organizada (tipo planilha) |
| coluna | um campo da planilha |
| linha / registro | um item guardado |
| query | um pedido de informação ao banco |
| índice | um atalho pra encontrar dados mais rápido |
| backup | cópia de segurança dos dados |
| SQL | a linguagem pra pedir coisas ao banco |
| PostgreSQL / MySQL / SQLite | tipos de banco, cada um com suas vantagens |
| ORM | ferramenta que deixa você mexer no banco sem escrever SQL |

### Autenticação & segurança

| Termo técnico | Tradução leiga |
|---|---|
| autenticação / auth | provar quem você é (login) |
| autorização | definir o que cada pessoa pode fazer |
| senha hasheada | senha guardada de um jeito que ninguém consegue ler de volta |
| JWT / token | um crachá digital que o sistema usa pra saber que é você |
| sessão | o tempo que você fica logado |
| cookie | um papelzinho que o site lê toda vez pra saber que é você |
| CORS | regra de quem pode chamar o sistema (tipo "só meusite.com.br") |
| HTTPS | conexão criptografada (cadeado no navegador) |
| SSL / TLS | a tecnologia por trás do cadeado |
| variável de ambiente | configuração guardada fora do código (senhas, chaves) |
| .env | arquivo onde ficam as configurações secretas |
| hash | embaralhar texto sem volta (pra guardar senha) |
| salt | ingrediente extra pro embaralhamento ser único |
| rate limit | limite de quantas vezes alguém pode pedir algo por minuto |
| XSS | ataque que injeta código malicioso no seu site |
| SQL injection | ataque que faz o banco executar comando perigoso |
| LGPD | lei brasileira de proteção de dados pessoais |
| API key | senha que identifica quem tá chamando um serviço externo |

### Servidor & infra

| Termo técnico | Tradução leiga |
|---|---|
| servidor | computador que fica ligado 24/7 respondendo pedidos |
| hospedagem | onde o sistema mora (servidor alugado) |
| VPS | servidor só seu numa máquina maior |
| domínio | o endereço bonito (meusite.com.br) em vez de IP |
| DNS | o que transforma "meusite.com.br" no número do servidor |
| CDN | cópia do seu site espalhada pelo mundo pra carregar rápido |
| cloud | servidores de outras pessoas que você aluga (AWS, Google Cloud, etc.) |
| container / Docker | caixinha que roda o sistema do mesmo jeito em qualquer lugar |
| orquestrador / Kubernetes | sistema que gerencia várias caixinhas rodando juntas |
| load balancer | distribui visitantes entre vários servidores |
| uptime | quanto tempo o sistema fica no ar sem cair |
| SLA | acordo de quanto tempo vai ficar no ar |
| observabilidade | conseguir ver o que tá acontecendo dentro do sistema |
| monitoria / monitoring | ficar de olho pra saber se caiu |

### Pagamentos & negócio

| Termo técnico | Tradução leiga |
|---|---|
| gateway de pagamento | empresa que processa cartão/Pix pra você |
| checkout | tela final de pagamento |
| assinatura recorrente | cobra todo mês / ano automaticamente |
| webhook | aviso automático que um serviço manda pro outro quando algo acontece |
| Stripe / PagSeguro / Mercado Pago | empresas que processam pagamento |
| Pix | transferência instantânea brasileira |
| split | dividir um pagamento entre várias pessoas |
| antifraude | sistema que tenta bloquear pagamento roubado |

### Loop Engineering (do paper Macedo)

| Termo técnico | Tradução leiga |
|---|---|
| loop | sistema que roda sozinho até terminar ou até dar limite |
| trigger | o que dispara a tarefa (botão, hora marcada, evento) |
| goal | o objetivo verificável ("termina quando X acontece") |
| verifier | o que checa se terminou de verdade |
| terminal state | o resultado final (deu certo / travou / acabou o limite) |
| budget | limite de quanto pode gastar (dinheiro, tempo, tentativas) |
| stagnation | quando não tá mais saindo do lugar |
| exhaustion | acabou o limite |
| especificação gaming | agente finge que terminou sem ter terminado |

### Processo VibeDev

| Termo técnico | Tradução leiga |
|---|---|
| gate | ponto de checagem obrigatório antes de avançar |
| Trilha Verde | caminho pra construir do zero |
| Trilha Vermelha | caminho pra consertar projeto existente |
| Fase | etapa do caminho |
| anti-escopo | lista do que NÃO entra (pra não crescer sem parar) |
| critério de pronto | descrição do que tem que acontecer pra considerar terminado |
| sub-tarefa | pedacinho do trabalho, com começo e fim |
| Tipo 1 (decisão) | escolha grande que é difícil mudar depois |
| Tipo 2 (decisão) | escolha pequena, dá pra trocar sem drama |
| Red Team | testar a decisão escolhida pra ver onde ela quebra |
| kill criteria | condição pra desistir do projeto inteiro |
| drift | sair do que tava planejado sem perceber |
| decision log | caderno de decisões importantes que não pode apagar |

---

## Termos que NÃO traduzir (são nomes próprios)

Mantém como está: Python, JavaScript, TypeScript, Node, React, Next.js, Vite,
FastAPI, Django, PostgreSQL, MongoDB, Redis, Supabase, Vercel, Netlify,
Railway, Render, Fly.io, AWS, GCP, Azure, Cloudflare, Auth0, Clerk,
Stripe, OpenAI, Anthropic, Claude, GPT, Codex, Cursor, GitHub, GitLab, Vercel,
npm, pnpm, yarn, pip, uv, Docker, Kubernetes, Terraform, etc.

---

## Como adicionar termo novo (regra pra IA)

Formato:

```markdown
| [novo termo técnico] | [tradução simples] |
```

Critério: a tradução tem que ser compreensível pra alguém que nunca escreveu
uma linha de código. Se você (IA) não consegue explicar sem usar outro termo
técnico, **simplifica mais** até conseguir.

Adicione na seção certa. Sem repetir entrada. Sem inflar sem necessidade.
Só traduzir o que aparecer de fato na conversa.
