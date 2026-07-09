# Modo País — Contexto Local Automático por País

> Detecção automática do país de operação baseado em sinais da sessão
> (idioma, moeda, fuso, domínio TLD, referências culturais). Carrega
> contexto local: idioma da UI, moeda, meios de pagamento, lei de
> proteção de dados, hospedagem, formato de data, fuso horário.

---

## ⚠️ IMPORTANTE — Disclaimer Obrigatório

**Este arquivo é ponto de partida pra conversas, não assessoria jurídica nem contábil.**

- Nada aqui substitui advogado, contador ou consultor de compliance local.
- Leis de proteção de dados mudam; gateways de pagamento mudam provedores; sanções internacionais mudam.
- **Antes de operar produto digital sério em qualquer país**, consulte profissional local.
- Para países marcados como `experimental` (CN, RU, UA), situação legal é **especialmente volátil** — verifique antes de tomar decisão.
- Este arquivo é mantido pela comunidade; última atualização: ver commit do git.

---

## Como funciona

### 1. Detecção automática

A cada mensagem da sessão, o framework checa **sinais combinados** — idioma sozinho não basta porque pt-BR ≠ pt-PT.

Sinais usados (em ordem de peso):

| Sinal | Peso | Exemplo |
|---|---|---|
| Domínio do projeto / menção de `.com.br`, `.pt`, `.ru` etc | Alto | `meuapp.com.br` → BR |
| Moeda explícita | Alto | `R$`, `€`, `₽`, `¥` |
| Referência cultural direta | Alto | "Pix", "MBWAY", "Alipay", "SPEI" |
| Fuso horário mencionado | Médio | GMT-3, GMT+8 |
| Idioma da conversa | Médio | pt-BR, pt-PT, ru-RU, zh-CN |
| Endereço / formato de telefone | Baixo | +55, +7, +86 |

**Resolução:** se sinais apontam pra **1 país** → ativa contexto.
Se sinais batem em **2+ países próximos** (ex: pt-BR e pt-PT) → pergunta ao usuário.

### 2. Opt-in explícito

Usuário pode forçar qualquer país:

- "ative modo Brasil" → força BR
- "tô no Japão" → ativa JP
- "modo Brasil off" → desativa

### 3. Output automático

Ao ativar, o framework aplica em todas as interações:

- Idioma da UI (em strings de exemplo)
- Moeda em toda simulação de custo
- Formato de data e número
- Fuso em exemplos de timestamp
- Pagamento: gateways locais primeiro
- Lei de proteção de dados: checagem ativa em segurança
- Hospedagem: provedores locais listados como opção

---

## Países suportados (12)

Cobertura atual. Cada um é uma seção abaixo.

### 🇧🇷 Brasil (BR)

- **Idioma da UI:** pt-BR
- **Moeda:** BRL (R$ X.XXX,XX)
- **Fuso:** GMT-3 (sem horário de verão desde 2019)
- **Datas:** DD/MM/AAAA
- **Pagamentos (ordem de sugestão):** Pix → Cartão (Stripe/Mercado Pago/Asaas/InfinitePay) → Boleto
- **Lei de dados:** LGPD (Lei 13.709/2018)
- **Hospedagem BR:** Locaweb, King.host, AWS sa-east-1, GCP southamerica-east1, Azure Brazil South
- **Sinais de detecção:** `.com.br`, "Pix", "Brasil", BRL, GMT-3, pt-BR com BR-sotaque ortográfico
- **Status:** Estável
- **Notas:** Pix é expectativa default de pagamento. LGPD se aplica a qualquer operação de dados pessoais de cidadãos brasileiros, mesmo com servidor fora do Brasil.

### 🇵🇹 Portugal (PT)

- **Idioma da UI:** pt-PT
- **Moeda:** EUR (€)
- **Fuso:** GMT+0 (continental: GMT+0/+1 com DST)
- **Datas:** DD/MM/AAAA
- **Pagamentos:** MBWAY → Multibanco → Cartão (Stripe/Adyen)
- **Lei de dados:** RGPD (Regulamento Geral de Proteção de Dados — versão UE) + Lei 58/2019 (execução nacional)
- **Hospedagem PT:** EUROtux, Sam Solutions, WebHS, Amazon eu-west-1 (Irlanda — suficiente pro GDPR)
- **Sinais de detecção:** `.pt`, pt-PT ortográfico (diferenças claras de pt-BR), MBWAY, "Portugal", GMT+0
- **Status:** Estável
- **Notas:** ortografia pt-PT ≠ pt-BR (ex: "facto" vs "fato", "utilizador" vs "usuário"). Atenção: modo Brasil carregado em projeto pt-PT vai soar errado.

### 🇺🇸 Estados Unidos (US)

- **Idioma da UI:** en-US
- **Moeda:** USD ($)
- **Fuso:** GMT-5 a GMT-10 (multi-fuso; usar GMT-5 NYC como referência)
- **Datas:** MM/DD/AAAA
- **Pagamentos:** Cartão (Stripe/Adyen/Square) → ACH → PayPal
- **Lei de dados:** sem lei federal única; CCPA (Califórnia), CPA (Colorado), VCDPA (Virginia) etc. Fragmentado por estado
- **Hospedagem:** AWS us-east-1, Vercel, Netlify, Render, Railway, Fly.io
- **Sinais de detecção:** `.com`, `.us`, USD, en-US, "ACH", "Stripe"
- **Status:** Estável
- **Notas:** fragmentação por estado é real — se coletar dados de californianos, CCPA aplica mesmo com empresa fora.

### 🇬🇧 Reino Unido (GB)

- **Idioma da UI:** en-GB
- **Moeda:** GBP (£)
- **Fuso:** GMT+0 (+1 com BST)
- **Datas:** DD/MM/AAAA
- **Pagamentos:** Cartão (Stripe) → Open Banking (TrueLayer) → Apple/Google Pay
- **Lei de dados:** UK GDPR + Data Protection Act 2018
- **Hospedagem UK:** AWS eu-west-2 (London), DigitalOcean LON1, Google Cloud europe-west2
- **Sinais de detecção:** `.co.uk`, `.uk`, GBP, "Sterling", GMT+0
- **Status:** Estável
- **Notas:** UK GDPR é juridicamente separado do EU GDPR pós-Brexit. Acordo de adequacy decision EU-UK existe.

### 🇲🇽 México (MX)

- **Idioma da UI:** es-MX
- **Moeda:** MXN ($ — mas atenção ao conflito visual com USD; usar "$X.XX MXN")
- **Fuso:** GMT-6
- **Datas:** DD/MM/AAAA
- **Pagamentos:** SPEI (transferência) → OXXO (pay-in-cash) → Cartão (Stripe/Mercado Pago/Clip)
- **Lei de dados:** LFPDPPP (Ley Federal de Protección de Datos Personales en Posesión de los Particulares)
- **Hospedagem MX:** AWS sa-east-1 (latência via São Paulo), Azure Mexico Central, Kio Networks
- **Sinais de detecção:** `.mx`, "RFC", "SPEI", "OXXO", es-MX, MXN
- **Status:** Estável
- **Notas:** OXXO é importante — atende público sem cartão. Mercado Pago e Clip são gateways BR-likes em escala.

### 🇦🇷 Argentina (AR)

- **Idioma da UI:** es-AR
- **Moeda:** ARS (volátil; muitas empresas cobram em USD paralelo)
- **Fuso:** GMT-3
- **Datas:** DD/MM/AAAA
- **Pagamentos:** Mercado Pago → Naranja X → Cartão
- **Lei de dados:** Ley 25.326 (Protección de los Datos Personales) + ARCA/AAIP
- **Hospedagem AR:** AWS sa-east-1, DonWeb, Dattatec
- **Sinais de detecção:** ".ar", "ARS", "Mercado Pago", es-AR com "vos" / "tenés"
- **Status:** Estável (cambial volátil)
- **Notas:** câmbio oficial vs blue é diferença grande. Sites costumam ter 2 preços. Não confunda com Chile/Colômbia mesmo sendo espanhol parecido.

### 🇨🇱 Chile (CL)

- **Idioma da UI:** es-CL
- **Moeda:** CLP ($ — usar $X.XXX CLP pra evitar conflito visual)
- **Fuso:** GMT-3 / GMT-4 (varia com DST)
- **Datas:** DD/MM/AAAA
- **Pagamentos:** Webpay (Transbank) → Khipu → Mercado Pago → Cartão
- **Lei de dados:** Ley 19.628 (modif. 2022)
- **Hospedagem CL:** AWS sa-east-1 (latência moderada), local: Hosting Plus, DreamHost Chile
- **Sinais de detecção:** `.cl`, "CLP", "Webpay", es-CL sotaque
- **Status:** Estável
- **Notas:** Webpay (Transbank) é dominante. Khipu é alternativa.

### 🇨🇳 China (CN)

- **Idioma da UI:** zh-CN (chinês simplificado)
- **Moeda:** CNY (¥)
- **Fuso:** GMT+8
- **Datas:** YYYY-MM-DD ou YYYY年MM月DD日
- **Pagamentos:** WeChat Pay → Alipay → UnionPay
- **Lei de dados:** PIPL (Personal Information Protection Law, 2021) + CSL (Cybersecurity Law, 2017) + DSL (Data Security Law, 2021)
- **Hospedagem CN:** obrigatória em solo chinês pra dados pessoais de cidadãos CN (cross-border com CAC approval)
- **Sinais de detecção:** `.cn`, `zh-CN`, CNY, "微信" (WeChat), "支付宝" (Alipay), "ICP备案" (ICP filing)
- **Status:** ⚠️ **Experimental** — regulação severa, exige certificação CAC pra serviços de IA generativa + ICP备案 pra qualquer site público + dados em solo chinês com avaliação de segurança.
- **Notas:** Para SaaS global com usuários chineses, complexidade regulatória é alta. Provedores estrangeiros (Stripe, Google) estão **bloqueados**. Sempre consultar advogado chinês e parceiro técnico local.

### 🇷🇺 Rússia (RU)

- **Idioma da UI:** ru-RU (russo + cirílico)
- **Moeda:** RUB (₽)
- **Fuso:** GMT+3 a GMT+12 (multi-fuso)
- **Datas:** DD.MM.YYYY
- **Pagamentos:** ЮKassa (YooKassa) → Тинькофф → Сбербанк Онлайн; **Visa/Mastercard suspenderam operações em 2022**
- **Lei de dados:** 152-ФЗ (Federal Law on Personal Data, 2006, endurecido em 2023/2024) — **dados de cidadãos RU devem estar em servidores em solo russo**
- **Hospedagem RU:** obrigatória solo RU; provedores: Yandex Cloud, VK Cloud, Selectel
- **Sinais de detecção:** `.ru`, `.рф`, "RUB", "₽", "152-ФЗ", "ЮKassa", "Тинькофф"
- **Status:** ⚠️ **Experimental** — sanções OFAC/EU afetam serviços internacionais (Stripe, AWS, Google Cloud com limitações); empresas ocidentais geralmente evitam exposição comercial ativa na Rússia.
- **Notas:** **Antes de processar pagamentos ou hospedar dados de cidadãos russos, verifique counsel local e compliance com sanções em jurisdição da sua empresa.** Este arquivo não orienta prática comercial ativa na Federação Russa dado contexto atual.

### 🇺🇦 Ucrânia (UA)

- **Idioma da UI:** uk-UA (ucraniano) + ru-UA (crescente mas politicamente sensível)
- **Moeda:** UAH (₴)
- **Fuso:** GMT+2 (+3 com DST em 2026?)
- **Datas:** DD.MM.YYYY
- **Pagamentos:** LiqPay (PrivatBank) → Monobank → WayForPay → Cartão (limitado por guerra)
- **Lei de dados:** Lei 2297-VI (Proteção de Dados Pessoais, 2010) — projeto de lei novo em discussão pra alinhar com GDPR
- **Hospedagem UA:** rara opção local; maioria hospeda em EU; AWS eu-central-1 (Frankfurt) ou eu-east-1 (Irlanda)
- **Sinais de detecção:** `.ua`, "UAH", "₴", "LiqPay", "Monobank", uk-UA
- **Status:** ⚠️ **Experimental** — país em conflito armado; economia parcialmente convertida; lei de PI em transição. Cuidado com contexto político.
- **Notas:** **Operar produto digital na Ucrânia em 2026 envolve contexto humanitário e regulatório volátil.** Empresas sérias contam com advisor local e decisões de principle.

### 🇪🇺 União Europeia (EU)

- **Idioma da UI:** varia por mercado-alvo (de, fr, es-ES, it, nl, pl, etc.)
- **Moeda:** EUR (€)
- **Fuso:** GMT+0 a GMT+2
- **Datas:** DD/MM/AAAA
- **Pagamentos:** SEPA → Cartão (Stripe/Adyen/Mollie) → Open Banking (PSD2)
- **Lei de dados:** GDPR (Regulamento (UE) 2016/679) + ePrivacy Directive pra cookies
- **Hospedagem EU:** AWS eu-central-1 (Frankfurt), eu-west-1 (Irlanda); região obrigatória pra muitos casos
- **Sinais de detecção:** `.eu`, EUR, "GDPR", "SEPA", "PSD2", idioma de país membro
- **Status:** Estável
- **Notas:** GDPR é **a referência global** em proteção de dados. Conformidade GDPR (+/- UK GDPR) é o "padrão ouro" mundial. Outras leis de PI em outros países geralmente apontam pra GDPR como benchmark.

### 🇯🇵 Japão (JP)

- **Idioma da UI:** ja-JP (japonês)
- **Moeda:** JPY (¥)
- **Fuso:** GMT+9
- **Datas:** YYYY/MM/DD
- **Pagamentos:** Cartão (Stripe) → Konbini (pay-in-convenience-store) → PayPay
- **Lei de dados:** APPI (Act on the Protection of Personal Information, 2003, endurecida em 2022)
- **Hospedagem JP:** AWS ap-northeast-1 (Tokyo), Google Cloud asia-northeast1, Sakura Cloud (local)
- **Sinais de detecção:** `.jp`, "JPY", "PayPay", "Konbini", ja-JP
- **Status:** Estável
- **Notas:** APPI 2022 endureceu consentimento + cross-border. Para app com usuários japoneses, transferências internacionais de dados exigem consentimento explícito ou adequacy decision equivalente.

---

## Matriz de decisão rápida

| Necessidade | Sugestão |
|---|---|
| SaaS BR pequeno | 🇧🇷 BR + pt-BR + BRL + Pix + LGPD |
| SaaS EUA pequeno | 🇺🇸 US + en-US + USD + Stripe + CCPA check |
| SaaS EU pequeno | 🇪🇺 EU + idioma-alvo + EUR + Stripe/Adyen + GDPR |
| App BR + internacionalização futura | 🇧🇷 BR primário, i18n desde o design |
| Chatbot/IA com público global | 🇪🇺 EU como base regulatória + i18n multi-idioma |
| App com público BR + vizinhança | 🇧🇷 BR + opcional 🇦🇷 AR/🇨🇱 CL/🇺🇾 (se expandir) |
| MVP experimental em país complexo | 🇪🇺 EU como "padrão GDPR" mesmo sediado em outro lugar |

---

## Detecção automática (lógica)

Algoritmo simplificado que o framework segue:

```
1. Coletar sinais: idioma, moeda, fuso, TLD, referências culturais
2. Pra cada país, calcular score = soma ponderada dos sinais detectados
3. País com maior score E score ≥ threshold (0.6) → ativa contexto
4. Empate entre 2+ países próximos → pergunta ao usuário
5. Score < 0.6 em todos → modo genérico (en-US + USD como default conservador)
6. Usuário pode override em qualquer ponto
```

---

## Disclaimer jurídico expandido

**O AVISO ABAIXO APLICA-SE A TODOS OS PAÍSES, ESPECIALMENTE AOS MARCADOS COMO EXPERIMENTAL:**

1. Este arquivo é mantido pela comunidade VibeDev sob licença MIT. Não constitui aconselhamento jurídico, contábil ou regulatório.
2. Leis de proteção de dados mudam; gateways de pagamento mudam termos; sanções internacionais mudam regimes.
3. Para qualquer produto que processa dados pessoais de cidadãos de país listado, **consulte advogado local** que atue com startups/produto digital.
4. Para gateways de pagamento, sempre verifique termos de uso atualizados do provedor.
5. Para países em conflito armado ou sob sanções (UA, RU), decisões legais exigem counsel especializado em comércio internacional.
6. Este arquivo é **compilado de fontes públicas** (políticas oficiais, Wikipedia, sites de provedores). Erros podem existir. Verifique antes de implementar.
7. Última atualização: ver git log do arquivo.

**Manter este arquivo atualizado é responsabilidade da comunidade.** PRs bem-vindos pra corrigir dados obsoletos ou adicionar países.
