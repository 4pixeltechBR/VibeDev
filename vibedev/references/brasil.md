# Modo Brasil — Contexto Local pra Projetos BR

> Prompts locais, referências regulatórias, meios de pagamento e idioma
> da UI. **Ativado automaticamente** quando a sessão é pt-BR ou quando
> há 1+ sinal brasileiro (BRL, Pix, Brasil, fuso GMT-3, projeto .com.br).
> Não polui a VibeDev principal — fica isolada aqui.

---

## Como ativar

### Automático
IA detecta pt-BR na conversa → carrega `references/brasil.md` → aplica.

### Explícito
Usuário fala "ativa modo Brasil" ou "usar regras brasileiras" → ativa.

### Opt-out
Usuário diz "modo Brasil off" ou tá em projeto pra outro país → desativa.

---

## O que o modo Brasil adiciona

### 1. Idioma da UI (pt-BR como default)
- Toda string visível pro usuário final vem em pt-BR.
- Mensagens de erro, e-mails transacionais, termos de uso.
- **Exceção:** código (variáveis, funções, comentários em inglês).

### 2. Moeda e formatação
- BRL como moeda de referência na tabela `custo_mensal_estimado`.
- Formatação: R$ 1.234,56 (vírgula, não ponto).
- Datas: DD/MM/AAAA.
- Fuso: GMT-3 (Brasília, sem horário de verão desde 2019).

### 3. Meios de pagamento
Quando o projeto envolve pagamento, sugerir nessa ordem:
1. **Pix** (zero taxa pra pessoa física, baixa taxa pra PJ)
2. **Cartão de crédito** via Stripe / Mercado Pago / InfinitePay / Asaas / Pagar.me
3. **Boleto** via mesma rede
4. **Link de pagamento** (InfinitePay, Mercado Pago) quando não quer integração complexa

**Recomendação por porte:**
- Side project / hobby: InfinitePay (sem mensalidade, % por venda)
- SaaS pequeno: Mercado Pago ou Asaas (API mais simples, suporte BR)
- SaaS em escala: Stripe + Pix (tem suporte oficial BR desde 2024)

**Cuidado:** Stripe BR é robusto mas caro em volume. Não é "default".

### 4. LGPD (Lei Geral de Proteção de Dados)

#### Classificação por tipo de projeto
| Tipo | LGPD se aplica? |
|---|---|
| Landing page sem formulário | Não (pode precisar de cookie banner se analytics) |
| App com login e-mail/senha | Sim — política de privacidade + aceite |
| App que coleta dados pessoais extras (CPF, endereço, saúde) | Sim + cuidados extras |
| App que processa pagamento | Sim + integração PCI-DSS se guardar cartão |
| Bot que conversa com pessoa | Sim — capturar consentimento |
| App mobile em loja | Sim — link de política visível |

#### Práticas mínimas obrigatórias
- **Política de privacidade** escrita em linguagem acessível, link visível
- **Termo de uso** linkado no signup
- **Cookie banner** se usar analytics (Google Analytics, Meta Pixel, Plausible)
- **Direito de exclusão** — usuário consegue pedir apagar os dados (LGPD art. 18)
- **Direito de exportação** — usuário consegue baixar os próprios dados (LGPD art. 18)
- **DPO / Encarregado** — obrigatório em alguns casos (empresa de médio/grande porte)

#### Cuidados específicos por tipo
- **Menores de idade:** consentimento dos pais (LGPD art. 11) — exige fluxo separado
- **Dados de saúde:** base legal específica (consentimento destacado)
- **Biometria / reconhecimento facial:** vedado pra criança, exige consentimento explícito
- **Localização:** só coletar com justificativa clara

#### Não-caia-nessa-armadilha
- "LGPD é só pra empresa grande" → **não é**. Aplica pra qualquer operação de dados pessoais no Brasil.
- "Avisei na política e tá ok" → **não é**. Coleta precisa de consentimento destacado, não escondido.
- "Site fora do Brasil não precisa" → **errado**. Se tem usuário BR coletando dado, aplica.

### 5. Hospedagem e dados em solo BR

#### Quando sugerir servidor no Brasil
- App publica que vai usar pra público BR (latência)
- Compliance do cliente exige dados no Brasil (contratos, financeiras)
- Quer pagar em BRL sem IOF de cloud gringa

#### Provedores
- **Locaweb, Hostgator BR, Uol Host** — hosting tradicional, painéis em pt-BR
- **King.host** — bom custo-benefício pra VPS
- **Laravel Forge BR alternativas (ServerSuperior, etc)** — paas BR
- **Render / Railway / Fly.io + região SP** — serverless/containers com região
- **DigitalOcean São Paulo** — quando precisa de VPS global com latência boa

#### Cloud global com região BR
- AWS (region sa-east-1, São Paulo)
- GCP (region southamerica-east1, São Paulo)
- Azure (region Brazil South)

**Atenção:** suporte em pt-BR varia muito. Nem sempre tem.

### 6. Contexto cultural / regulatório específico

#### Pix como default
A pessoa brasileira espera Pix. Se o app cobra e não tem Pix, pergunta "cadê o Pix?". Sugerir Pix em qualquer checkout é regra local.

#### CNPJ vs CPF
- **PJ (CNPJ):** InfinitePay, Asaas, Mercado Pago, Cobre Fácil
- **PF (CPF):** os mesmos, mas注意 com limite (Pix pessoa física noturna tem limite Banco Central)

#### Nota fiscal
App que vende serviço/produto precisa emitir NF-e. Lembrar:
- **MEI:** pode emitir NF-e pelo MEI Fácil (grátis, gov.br)
- **Simples Nacional:** precisa de emissor (NF-e Paulista, FocusNFe, etc)
- Acima de R$ 80k/ano ou certos serviços, CPF não dá

#### Fuso e horários de pico
- Pico de uso brasileiro é 19h-22h GMT-3 (horário de jantar)
- Suporte / atendimento é esperado em pt-BR
- "Madrugada" pra americano é horário comercial pra BR (10h GMT-3 = 6h NY)

### 7. Idioma de programação/código
- Variáveis, funções, classes, arquivos: **inglês** (convenção universal)
- Mensagens ao usuário: **pt-BR**
- Comentários: **português**, a menos que seja projeto open-source global
- README do projeto: **bilíngue** (pt-BR primeiro, en opcional)

---

## Como IA usa isso (regra de execução)

Quando modo Brasil tá ativo:

1. Toda simulação de custo usa BRL como moeda primária.
2. Toda sugestão de pagamento pergunta se quer Pix antes de cartão.
3. Toda checagem de segurança (via VibeShield) inclui LGPD explicitamente.
4. Toda sugestão de hosting pergunta se precisa solo BR.
5. Toda UI string mostrada em exemplo é em pt-BR.
6. Toda data mostrada é DD/MM/AAAA.
7. Toda moeda é R$ X.XXX,XX.
8. Toda menção de horário usa GMT-3 (Brasília).

---

## Quando **NÃO** ativar

- Projeto pra público fora do Brasil
- Cliente internacional
- Projeto open-source global
- Time trabalha em inglês

Detecção: se o usuário tá conversando 100% em inglês e nunca menciona
Brasil, modo Brasil fica off por default.

---

## Aviso importante

Isso **não é assessoria jurídica ou contábil**. LGPD e tributação são
complexos. Para projeto sério (volume médio/alto de usuários, dados
sensíveis, cobrança recorrente), **contrate advogado e contador** que
atuam com startups/produto digital.

Este arquivo é **ponto de partida pra perguntas certas**, não checklist
de conformidade.
