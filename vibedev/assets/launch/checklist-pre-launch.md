# Checklist pré-lançamento — 24h antes

> Use este checklist na véspera do lançamento. Não publica nada sem bater ele.

---

## Legal & Compliance

- [ ] Política de privacidade escrita, linkada no footer
- [ ] Termo de uso linkado no signup / compra
- [ ] Cookie banner ativo se houver analytics
- [ ] LGPD: fluxo de "esqueci meus dados / apagar minha conta" funciona
- [ ] LGPD: fluxo de "exportar meus dados" funciona
- [ ] Formas de contato claras (e-mail / formulário)
- [ ] CNPJ / Razão social visível (se for o caso)
- [ ] Nota fiscal configurada ou aviso de isenção

## Segurança

- [ ] HTTPS ativo (cadeado no navegador)
- [ ] HSTS configurado (force HTTPS)
- [ ] Senhas hasheadas (não em texto puro)
- [ ] Variáveis sensíveis (.env) fora do git
- [ ] CORS configurado (não aberto pra qualquer origem)
- [ ] Rate limit ativo em login / signup / formulários públicos
- [ ] Logs não expõem dados pessoais
- [ ] Backup do banco funcionando e sendo testado

## Funcionalidade

- [ ] Fluxo principal (descrito na Fase 1) funciona ponta-a-ponta
- [ ] Login / Signup / Logout testados em 2 navegadores diferentes
- [ ] Mobile: testou pelo celular (não só DevTools)
- [ ] Erro 404 tem página customizada (não mostra stack trace)
- [ ] Pagamento: testou com cartão teste + Pix teste
- [ ] E-mails transacionais (signup, recovery) chegam e não caem em spam

## Performance

- [ ] Lighthouse / PageSpeed ≥ 80 mobile
- [ ] Imagens comprimidas (não 5MB de upload)
- [ ] Banco indexado em queries lentas
- [ ] CDN ativo para assets públicos

## Conteúdo

- [ ] Copy revisada em todas as páginas principais
- [ ] Sem texto "Lorem ipsum" no ar
- [ ] SEO básico: title, description, og:image configurados
- [ ] Favicon
- [ ] Robots.txt e sitemap.xml

## Operação

- [ ] Domínio registrado e renovando automaticamente
- [ ] Monitoramento básico (uptime — pode ser gratuito tipo UptimeRobot)
- [ ] Log de erro visível (Sentry free tier já serve)
- [ ] Plano de rollback: sabe voltar a versão anterior?
- [ ] Dinheiro: cartão cadastrado na hospedagem, com alerta de cobrança

## Comunicação

- [ ] Primeiro post agendado pra amanhã
- [ ] Primeiros 10 usuários já notificados pessoalmente
- [ ] Material de suporte preparado (FAQ ou e-mail)

---

## Notas de lançamento (preencher no dia)

- **Data/hora do lançamento:** 
- **Versão que entrou no ar:** 
- **Número de beta users no D-0:** 
- **Principal receio pós-lançamento:** 
- **Primeira coisa que vou olhar nas próximas 24h:** 

---

## Depois do lançamento

Monitorar por 7 dias consecutivos:

- Quantas visitas
- Quantos cadastros (taxa de conversão)
- Quantos chegaram no fluxo principal
- Quantos pediram suporte / reclamaram
- Quanto de fato tá saindo do bolso

Se nas primeiras 48h **nenhum feedback chega** → você lançou sem beta
testing. Próximo lançamento precisa de coorte beta maior antes.
