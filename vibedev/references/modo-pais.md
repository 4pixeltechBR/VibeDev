# Modo País — Contexto Local Automático por País

> Detecção automática do país do **desenvolvedor** baseado em sinais da
> sessão (idioma, moeda, fuso, domínio TLD, referências culturais).
> Carrega contexto local: idioma da UI, moeda, meios de pagamento,
> lei de proteção de dados, hospedagem, formato de data, fuso horário.

---

## Propósito

**Este arquivo serve devs nativos de cada país** que querem construir
ferramentas digitais pro próprio país — com gateways locais, servidores
locais, leis locais, idioma local. Tecnologia cívica: ajudar quem mora
lá a resolver problemas lá, com infra local.

Não é guia pra empresas estrangeiras operando em outros mercados. Quem
lê um país que não é o seu geralmente tá só curioso — tá tudo bem,
mas não use a seção alheia pra operar.

## Modelo mental

- Você tá em país X, fala idioma X, usa moeda X, quer construir app pro público X.
- O `modo-pais.md` carrega o que é útil pra isso: gateways de pagamento
  que funcionam em X, hospedagem que cumpre a lei de X, formato de UI
  que faz sentido em X, links pras leis de proteção de dados de X.
- Você não precisa aprender nada de Y, Z, W. Vai direto pro que serve.

## Cobertura

12 países: 🇧🇷 BR, 🇵🇹 PT, 🇺🇸 US, 🇬🇧 GB, 🇲🇽 MX, 🇦🇷 AR, 🇨🇱 CL, 🇨🇳 CN, 🇷🇺 RU, 🇺🇦 UA, 🇪🇺 EU, 🇯🇵 JP.

Cada país tem contribuidor identificado ou **apelação aberta** (PRs
bem-vindos — ver seção "Contribuindo").

## Aviso jurídico (vale pra todos)

- Este arquivo é ponto de partida, **não assessoria jurídica nem contábil**.
- Antes de qualquer operação séria (pagamento recorrente, coleta de dados
  pessoais de clientes, emissão de nota fiscal / invoice / fapiao), consulte
  **profissional local** do país em questão.
- Leis mudam. Gateways mudam termos. Status (estável / experimental) é
  opinião da comunidade, não garantia legal.
- Última atualização: ver commit do git.

## Aviso específico pra empresas estrangeiras

Se você é dev/empresa **fora** do país alvo e quer operar lá comercialmente,
este arquivo **não é seu guia**. Você enfrentará sanções internacionais,
compliance extra, e barreiras regulatorias fora do escopo. Contrate
advogado local + consultor de expansão internacional.

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

Você tá construindo app pra público chinês, rodando em servidores chineses, com gateways chineses.

- **Idioma da UI:** zh-CN (chinês simplificado; tradicional pra Taiwan/HK/Macau)
- **Moeda:** CNY (¥) ou CNH (offshore)
- **Fuso:** GMT+8 (Horário de Pequim, único fuso nacional)
- **Datas:** YYYY-MM-DD (ISO) ou YYYY年MM月DD日 (texto); semana inicia na segunda
- **Pagamentos (dev local):** WeChat Pay (微信支付) → Alipay (支付宝) → UnionPay (银联) → JD Pay
  - **Integração:** WeChat Pay Open Platform + Alipay Open Platform — ambas têm SDK e docs em chinês, suporte a sandbox
  - **Atenção:** Stripe, PayPal, Adyen **não estão disponíveis** dentro do ecossistema chinês
- **Lei de dados:** PIPL (个人信息保护法, 2021) + CSL (网络安全法, 2017) + DSL (数据安全法, 2021)
  - Pra processar dados pessoais de cidadãos chineses: precisa de consentimento granular
  - Cross-border data transfer: exige CAC security assessment ou standard contract
  - Serviços de IA generativa: precisa de algoritmo备案 (filing) com CAC
- **Hospedagem (China-based, recomendado):**
  - **Aliyun (阿里云)** — maior provedor, presença em todas as regiões
  - **Tencent Cloud (腾讯云)** — forte em gaming e mensageria
  - **Huawei Cloud (华为云)** — opção state-backed
  - **Baidu Cloud** — bom em AI/ML
- **ICP备案 (filing obrigatório):** qualquer site público em servidor chinês precisa de ICP备案 — registro único, leva 7-20 dias úteis. CN (company) ou IDC (individual) variant.
- **Provedores internacionais:** geralmente acessíveis de dentro do país pra uso em desenvolvimento, mas **pagamento a clientes finais em CNY** exige integração com gateway local
- **Sinais de detecção:** `.cn`, `zh-CN`, CNY, "微信" / "支付宝" / "备案" / "ICP"
- **Status:** Estável (manutenção comunitária; lacunas marcadas abaixo)

**Lacunas conhecidas — contribuidor local bem-vindo:**
- Lista atualizada de gateways menores (Douyin Pay, Kuaishou Pay) — quem mora lá sabe quem domina em cada nicho
- Procedimento detalhado de ICP备案 step-by-step em 2026 (regulamentação muda)
- Procedimento CAC security assessment pra cross-border
- SDKs Node/Python/Java com links e versões atuais
- Sub-cenários: HK/Macau (idioma zh-HK/zh-MO, lei diferente), Taiwan (zh-TW)

### 🇷🇺 Rússia (RU)

Você tá construindo app pra público russo, rodando em servidores russos, com gateways russos.

- **Idioma da UI:** ru-RU (russo, alfabeto cirílico)
- **Moeda:** RUB (₽)
- **Fuso:** GMT+3 a GMT+12 (multi-fuso, 11 fusos; Kaliningrado GMT+2, Vladivostok GMT+10)
- **Datas:** DD.MM.YYYY
- **Pagamentos (dev local):** ЮKassa (YooKassa) → Тинькофф (Tinkoff) Касса → СберБизнес (SberBusiness) → Robokassa
  - **Cartões internacionais:** Visa/Mastercard **suspenderam operações** em território russo desde 2022. Cartões russos (МИР - Mir) dominam.
  - **Atenção pra dev:** ao testar com cartão de teste, busque cartões Mir de teste, não Visa/MC
- **Lei de dados:** 152-ФЗ (Федеральный закон "О персональных данных", 2006 — endurecido em 2023/2024)
  - **Solo russo obrigatório:** dados pessoais de cidadãos russos devem ser processados em servidores fisicamente em território russo
  - Antes de lançar: registrar como operador de dados pessoais no РосКомСвобода (Roskomnadzor) — орган regulador
  - **Cross-border transfer:** proibido pra maioria de dados pessoais sem consentimento específico ou cláusula contratual aprovada
- **Hospedagem (Russia-based):**
  - **Yandex Cloud** — maior, integra com resto do ecossistema Yandex
  - **VK Cloud (Mail.ru Cloud Solutions)** — forte em serviços managed
  - **Selectel** — bom em VPS dedicado e armazenamento
  - **Timeweb Cloud** — opção acessível pra MVP / hobby
- **Serviços estrangeiros (Stripe, AWS, Google Cloud):** acessíveis dentro do país pra desenvolvimento, mas pagamento a clientes finais em RUB exige integração local.
- **Sinais de detecção:** `.ru`/`.рф`, "₽", "RUB", "ЮKassa" / "Тинькофф" / "Сбер" / "152-ФЗ"
- **Status:** Estável (manutenção comunitária; lacunas abaixo)

**Lacunas conhecidas — contribuidor local bem-vindo:**
- Procedimento atual de registro no Roskomnadzor (órgão regulador) — muda com frequência
- Lista de provedores menores / regionais (RUVDS, Aéza, FastVPS)
- SDK Node/Python/Java pra YooKassa + Tinkoff — links e versões atuais
- Sub-cenários: Bielo-Rússia (be-BY, BYN), Cazaquistão (kz-KK, KZT) com legislação correlata

### 🇺🇦 Ucrânia (UA)

- **Idioma da UI:** uk-UA (ucraniano). Algumas regiões usam russo mas contexto político torna uk-UA o default recomendado.
- **Moeda:** UAH (₴)
- **Fuso:** GMT+2 (EET) / GMT+3 (EEST com DST)
- **Datas:** DD.MM.YYYY
- **Pagamentos (dev local):** LiqPay (PrivatBank) → Monobank → WayForPay → Fondy → Portmone
  - **Cartões:** Visa/MC funcionam em território ucraniano. Stripe/Adyen acessíveis.
  - **Atenção:** contexto bélico significa algumas rotas internacionais podem oscilar
- **Lei de dados:** Lei 2297-VI (Закон "Про захист персональних даних", 2010)
  - projeto de lei novo em discussão pra alinhar com GDPR (status atual: consultar)
  - Para coleta de dados pessoais: base legal (consentimento, contrato, obrigação legal) e notificação ao Уповноважений (Comissário de Direitos Humanos)
  - **Cross-border transfer:** permitido com garantias adequadas
- **Hospedagem (Ukraine-based, opção limitada):**
  - **Data.center Ukraine (DCUA), GigaCenter, Colocall** — DCs locais; oferta limitada
  - **Maioria hospeda em EU** — AWS eu-central-1 (Frankfurt), eu-east-1 (Ireland), GCP europe-west, Azure West Europe
  - Justificativa comum: lei local permite hosting fora com garantias contratuais
- **Serviços estrangeiros:** plenamente acessíveis. Stripe, AWS, Google Cloud funcionam normalmente.
- **Sinais de detecção:** `.ua`, "UAH", "₴", "LiqPay" / "Monobank" / "WayForPay" / "ПриватБанк"
- **Status:** Estável (manutenção comunitária; lacunas abaixo)

**Lacunas conhecidas — contribuidor local bem-vindo:**
- Status atual do projeto de lei novo (alinhamento GDPR) — quem mora lá acompanha
- Procedimento de notificação ao Уповноважений (Comissário)
- Lista de DCs ucrânianos menores e seus SLAs
- SDKs de gateways — versões e links atualizados
- Operação sob conflito: backup geo-redundante, plano de continuidade, escolha de DC resistente

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

| Você é dev em... | Construindo pra... | Sugestão |
|---|---|---|
| 🇧🇷 BR | público brasileiro | 🇧🇷 BR + pt-BR + BRL + Pix + LGPD |
| 🇵🇹 PT | público português | 🇵🇹 PT + pt-PT + EUR + MBWAY + RGPD |
| 🇺🇸 US | público americano | 🇺🇸 US + en-US + USD + Stripe + CCPA |
| 🇬🇧 GB | público britânico | 🇬🇧 GB + en-GB + GBP + Stripe + UK GDPR |
| 🇲🇽 MX | público mexicano | 🇲🇽 MX + es-MX + MXN + SPEI/OXXO + LFPDPPP |
| 🇦🇷 AR | público argentino | 🇦🇷 AR + es-AR + ARS + Mercado Pago + Ley 25.326 |
| 🇨🇱 CL | público chileno | 🇨🇱 CL + es-CL + CLP + Webpay + Ley 19.628 |
| 🇨🇳 CN | público chinês | 🇨🇳 CN + zh-CN + CNY + WeChat/Alipay + PIPL |
| 🇷🇺 RU | público russo | 🇷🇺 RU + ru-RU + RUB + ЮKassa + 152-ФЗ |
| 🇺🇦 UA | público ucraniano | 🇺🇦 UA + uk-UA + UAH + LiqPay + Lei 2297-VI |
| 🇪🇺 EU | público europeu (genérico) | 🇪🇺 EU + idioma-alvo + EUR + Stripe/Adyen + GDPR |
| 🇯🇵 JP | público japonês | 🇯🇵 JP + ja-JP + JPY + PayPay + APPI |

**Cenários especiais:**
- App expandindo de 1 país pra outro: comece pelo país de origem, i18n desde o design (use chaves de tradução, armazene moeda como ISO 4217, datas ISO 8601)
- Chatbot/IA multi-país: identifique mercado-alvo primário (origem do time) e adicione suporte gradual
- Operação comercial de empresa estrangeira num país não sede: **fora do escopo deste arquivo**, procure consultor de expansão internacional

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

## Contribuindo — chamado a maintainers nativos

Todos os 12 países deste arquivo podem ser melhorados por quem **mora lá**.

### Como ajudar (passo a passo)

1. **Fork** o repo `4pixeltechBR/VibeDev`
2. Edite `vibedev/references/modo-pais.md`:
   - Corrija links quebrados, status de provedores, datas de leis
   - Adicione gateways / hosts menores que conhece do seu mercado
   - Atualize regulação se mudou
   - Se errou algo, conserte — melhor pedir perdão que pedir permissão
3. PR com descrição curta do que mudou e por quê
4. Tag gente nativa do país se puder revisar

### Países que mais precisam de contribuidor local

**🇨🇳 China:** leis mudam rápido (PIPL gerou várias regulamentações
secundárias desde 2021); SDKs de WeChat Pay e Alipay ficam desatualizados
em meses. Se você é dev chinês, cada PR ajuda dezenas de devs.

**🇷🇺 Rússia:** cenário regulatório pós-2022 é volátil. Roskomnadzor
publica novas diretrizes. Se você trabalha com dev russo, conhece
YooKassa na prática, sabe quem tá usando VK Cloud vs Yandex Cloud,
sua contribuição é de alto valor.

**🇺🇦 Ucrânia:** projeto de lei novo de PI em discussão. Status atual
de DCs sob conflito. SDKs de gateways funcionando sob condições de
guerra são coisas que só quem tá lá sabe.

**Outros países também:**
- **🇲🇽 MX:** OXXO ainda é subdocumentado fora.
- **🇦🇷 AR:** câmbio volátil — quem usa Mercado Pago lá sabe nuances que documentação oficial não conta.
- **🇨🇱 CL:** Webpay tem nuances técnicas que só integrador local pega.
- **🇯🇵 JP:** Konbini + PayPay têm fluxos específicos; APPI 2022 endureceu consentimento.

### Como detectar contribuição prioritária

Cada país já marca no fim do seu bloco a seção:

```
**Lacunas conhecidas — contribuidor local bem-vindo:**
- [lista de pontos a endurecer]
```

PR que ataque qualquer item da lista é especialmente bem-vindo.

## Disclaimer jurídico final

**Vale pra todos os 12 países:**

1. Este arquivo é mantido pela comunidade VibeDev sob licença MIT. **Não** constitui aconselhamento jurídico, contábil ou regulatório.
2. Leis de proteção de dados mudam; gateways mudam termos; regulação evolui.
3. Para qualquer produto que processe dados pessoais de cidadãos do país, **consulte advogado local** que atue com startups/produto digital.
4. Para gateways de pagamento, sempre verifique termos de uso atualizados direto na documentação do provedor.
5. Este arquivo é **compilado de fontes públicas** (políticas oficiais, Wikipedia, sites de provedores). Erros podem existir. Verifique antes de implementar.
6. Última atualização: ver git log do arquivo.

**Última frase, importante:** se você tá num país listado e o framework tá
te ajudando a construir uma ferramenta pro seu próprio povo, ótimo.
Continue. Contribua quando puder. É assim que a coisa cresce.
