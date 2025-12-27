# QUESTÕES ABERTAS E RISCOS

**Incógnitas inevitáveis que requerem decisões futuras**

[fonte: 00_README.md → Questões Abertas]

---

## 1. INTEGRAÇÕES EXTERNAS

### ElevenLabs TTS
**Questão**: Como gerenciar custo em escala?
**Risco**: Uso massivo pode explodir custo
**Mitigação IMPLEMENTADA** (Resposta Cliente #1):
- **Cache inteligente**: Gera áudio UMA VEZ no primeiro play, depois serve cached
- Cache é GLOBAL (mesmo áudio para todos os usuários)
- NÃO regenera áudio a menos que criador atualize conteúdo (invalida cache)
- Quotas por plano (MicroSaaS: 1k chars, Standard: 5k, Full: 20k)
- Alertas quando quota > 80%

**Decisão RESOLVIDA**: Sistema de cache elimina regenerações. Fallback: Bloquear geração se quota estoura + alerta ao criador para upgrade.

[fonte: Resposta Cliente #1 → Sistema de cache inteligente - gera UMA VEZ, depois cached]

---

### Assets/Storage
**Questão**: Como armazenar assets (imagens/SVG/vídeos) em escala?
**Risco**: Storage pode crescer rapidamente
**Mitigação proposta**:
- Limites por plano (MicroSaaS: 1GB, Standard: 5GB, Full: 50GB)
- Compressão automática de imagens
- Cleanup de assets não usados (após X dias)

**Decisão pendente**: CDN obrigatório desde v1? Ou apenas quando atingir escala?

---

## 2. PROVA DE APLICAÇÃO (Anti-Fraude)

**Questão**: Como validar aplicação sem upload de arquivos?
**Decisão RESOLVIDA** (Resposta Cliente #2):
- **SEM upload de arquivos/screenshots**
- Validação baseada em **honestidade + checklist auto-declarativo**
- Aluno preenche texto (descrição) OU link (opcional)
- Anti-fraude: Detecção de padrões suspeitos (texto copiado, links spam)

**Risco**: Fraude aumenta com auto-declaração, mas cliente prefere evitar fricção de upload.

**Nova questão**: Como validar honestidade? (ver arquivo 12_DUVIDAS_VALIDACAO/duvidas_para_cliente.md)

[fonte: Resposta Cliente #2 → REMOVER ou SIMPLIFICAR upload - evitar tarefas de upload]

---

## 3. MARKETPLACE → LOJA DE RECOMPENSAS

**MUDANÇA FUNDAMENTAL** (Resposta Cliente #3):
- **Marketplace NÃO é catálogo de apps criados**
- **Marketplace É loja de recompensas**: Aluno troca XP/moedas por produtos/benefícios
- Estética Amazon/Mercado Livre simplificada
- Sempre 2 opções: "Comprar agora" (dinheiro real) ou "Comprar com Coins" (moedas)

**Implicações**:
- Platform Admin configura produtos da loja (não curadora apps)
- Learner acessa loja para gastar moedas
- Creator Studio pode ativar/desativar loja no seu app
- NÃO há discovery de InfoApps em v1 (isso vai para v1.5)

**Nova questão**: Quem configura produtos? Platform Admin ou Creator? (ver arquivo 12_DUVIDAS_VALIDACAO/)

[fonte: Resposta Cliente #3 → Marketplace é LOJA DE RECOMPENSAS]

---

## 4. NOTIFICAÇÕES PUSH (Retenção)

**Decisão RESOLVIDA** (Resposta Cliente #4):
- **Notificações Push: v1.1** (não v1)
- Se houver solução GRATUITA viável, pode entrar em v1.1
- v1: Email notificações apenas (streak em risco, novas missões)

**Risco**: Retenção D1/D7 pode ser mais baixa sem push, mas cliente aceita isso em v1.

**Serviço sugerido v1.1**: OneSignal (free tier) ou Firebase Cloud Messaging (free)

[fonte: Resposta Cliente #4 → Solução gratuita ou deixar v1.1]

---

## 5. DARK MODE

**Decisão RESOLVIDA** (Resposta Cliente #5):
- **Dark mode: v1.1** (não v1)
- v1: Light mode apenas
- Design System deve preparar tokens para facilitar implementação futura

**Mitigação**:
- Tokens de design já pensados para swap de cores
- Media query `prefers-color-scheme: dark` preparado (mas não ativo em v1)

[fonte: Resposta Cliente #5 → Deixar para v1.1]

---

## 6. MULTILINGUAGEM (i18n)

**Decisão RESOLVIDA** (Resposta Cliente #6):
- **i18n: SIM, criar já pensando em suportar múltiplos idiomas**
- v1: Infraestrutura de i18n implementada, idioma inicial PT-BR
- v1.1+: Adicionar EN, ES

**Implementação**:
- Biblioteca de i18n (react-i18next ou next-intl)
- Todos os textos da interface via chaves de tradução
- Conteúdo criado pelo Creator: 1 idioma em v1 (questão: criador traduz ou automático?)

**Nova questão**: Conteúdo do criador é multi-idioma? (ver arquivo 12_DUVIDAS_VALIDACAO/)

[fonte: Resposta Cliente #6 → SIM, criar já pensando em suportar múltiplos idiomas]

---

## 7. MOBILE (Native vs Web)

**Decisão RESOLVIDA** (Resposta Cliente #7):
- **v1: PWA web responsivo** (NÃO app native)
- Otimizado para migração futura para native (estrutura de componentes, APIs, etc)
- PWA installable: SIM (manifest.json, service worker básico)

**Implementação**:
- Framework web (React/Next.js ou similar)
- Design responsivo mobile-first
- PWA manifest + service worker para instalação
- Migração para React Native/Flutter: v2+ (quando atingir tração)

[fonte: Resposta Cliente #7 → PWA web responsivo, otimizado para migração futura]

---

## 8. OFFLINE MODE

**Decisão RESOLVIDA** (Resposta Cliente #8):
- **Offline mode: NÃO precisa** (nem v1, nem v1.1)
- Produto assume conexão online

**Justificativa**: Simplifica arquitetura (não precisa sync, cache de assets, etc)

**Risco aceito**: Usuários em conexões ruins podem ter experiência degradada.

[fonte: Resposta Cliente #8 → NÃO precisa offline]

---

## 9. CHECKOUT/BILLING (Aluno → Criador)

**Decisão RESOLVIDA** (Resposta Cliente #9):
- **Checkout/Billing: v1.5** (NÃO v1)
- v1: NÃO há pagamento de aluno para criador
- v1: Apenas planos de assinatura Platform (MicroSaaS/Standard/Full) para criadores
- v1.5: Implementar billing de aluno → criador (monetização de apps)

**Plataforma sugerida v1.5**: Stripe (Stripe Connect para marketplace)

[fonte: Resposta Cliente #9 → Checkout/Billing v1.5]

---

## 10. SEO/DISCOVERY (Apps Criados)

**Decisão RESOLVIDA** (Resposta Cliente #10):
- **SEO/Discovery: v1.5** (NÃO v1)
- v1: Cada produto (app criado) tem seu funil próprio (sem discovery centralizado)
- v1.5: Implementar discovery/marketplace de apps + SEO avançado

**Implicação v1**:
- Apps criados são acessados via link direto ou domínio personalizado
- Sem busca/filtros de apps públicos
- Sem landing pages SEO-friendly

**v1.5**:
- SSR/SSG para SEO (Next.js ou similar)
- Marketplace público de apps com busca/filtros
- Featured apps, categorias, ratings

[fonte: Resposta Cliente #10 → SEO/Discovery v1.5, cada produto tem funil próprio]

---

## 11. COMPLIANCE (LGPD/GDPR)

**Decisão RESOLVIDA** (Resposta Cliente #11):
- **LGPD: SIM, está coberto**
- Assinantes autorizam coleta/uso de dados (consentimento explícito)
- Política de privacidade clara
- Direito de exclusão de dados implementado

**Implementação v1**:
- Signup: Checkbox de consentimento LGPD obrigatório
- Settings: Opção "Excluir minha conta" (GDPR Article 17)
- Política de privacidade acessível

**Revisão legal**: Recomendado antes de v1 (validar termos + política de privacidade)

[fonte: Resposta Cliente #11 → SIM, está coberto (assinantes autorizam)]

---

## 12. INFRA/DEVOPS

**Decisão DELEGADA** (Resposta Cliente #12):
- **Time deve definir solução mais adequada**
- Cliente confia na escolha técnica do time

**Recomendação**:
- v1: Vercel/Netlify (simplicidade, deploy fácil, PWA-friendly)
- v1+: Migrar para AWS/GCP se necessário (escala, controle, custos)
- CI/CD: GitHub Actions ou similar desde v1

**Critérios de escolha**:
- Custo inicial baixo
- Deploy fácil (time pequeno)
- Escalável (quando crescer)
- PWA-friendly (service workers, HTTPS)

[fonte: Resposta Cliente #12 → Time deve definir solução mais adequada]

---

## 13. ESCALABILIDADE (80+ APPS) + ARQUITETURA MULTI-APP

**Decisão DELEGADA + ESCLARECIDA** (Respostas Cliente #13 e #14):
- **Time escolhe solução mais adequada**
- **Modo Studio é a BASE** de todos os apps criados
- **Cada app criado tem estrutura própria**: subdomínio/domínio personalizado/URL
- **NÃO precisa ser todos na mesma estrutura** (permite isolamento por app)

**Arquitetura sugerida**:
- Multi-tenancy: 1 workspace = 1 marca/criador
- Cada InfoApp criado: Subdomínio automático (ex: app123.plataforma.com) OU domínio custom
- Banco de dados: Isolado por workspace (ou shared com tenant_id)
- Cache: Redis por app ou global
- CDN: CloudFlare ou similar para assets

**Load testing**: v1.1 (após primeiros apps em produção)

**Nova questão**: Apps compartilham banco ou são isolados? (ver arquivo 12_DUVIDAS_VALIDACAO/)

[fonte: Resposta Cliente #13 → Modo Studio é BASE, cada app tem estrutura própria]
[fonte: Resposta Cliente #14 → Time escolhe solução de escalabilidade]

---

## 14. ACESSIBILIDADE (WCAG AA)

**Decisão MANTIDA** (já estava no conselho):
- **WCAG AA é obrigatório desde v1**
- Contrast ratio mínimo (4.5:1)
- Focus visível sempre
- ARIA labels
- Reduce motion/sound/haptics (toggles em Settings)

**Auditoria**: v1 (ferramentas automatizadas: axe, Lighthouse) + v1.1 (auditoria manual)

**Risco**: Acessibilidade mal implementada em v1 gera dívida técnica impossível de consertar depois.

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Accessibility Lead → "Consertar agora, não depois"]

---

## 15. ANALYTICS (Ferramental)

**Decisão RESOLVIDA** (Resposta Cliente #15):
- **Solução gratuita ou simples** em v1
- Pode ser trocado depois (via abstração de eventos)

**Opções gratuitas/simples v1**:
- Google Analytics 4 (gratuito, básico)
- Plausible (open-source, simples, privacy-friendly)
- PostHog (free tier generoso)

**Implementação**:
- Abstração de eventos (camada única de tracking)
- Taxonomia de eventos documentada (já feito em 07_ANALYTICS_LIGHT/)
- Dashboard custom no Creator Studio: v1 (básico) ou v1.1 (avançado)

**Nova questão**: Dashboard custom no Creator Studio ou link externo? (ver arquivo 12_DUVIDAS_VALIDACAO/)

[fonte: Resposta Cliente #15 → Solução gratuita ou simples]

---

## RESUMO: DECISÕES RESOLVIDAS vs PENDENTES

### ✅ DECISÕES RESOLVIDAS (Respostas Cliente)

**v1 (Core)**:
1. ✅ TTS: Cache inteligente (gera UMA vez, serve cached)
2. ✅ Aplicação: SEM upload (checklist auto-declarativo + texto/link)
3. ✅ Marketplace: LOJA DE RECOMPENSAS (não catálogo de apps)
4. ✅ Mobile: PWA web responsivo (não native)
5. ✅ Offline: NÃO precisa
6. ✅ i18n: SIM, infraestrutura desde v1 (PT-BR inicial)
7. ✅ LGPD: Coberto (consentimentos obrigatórios)
8. ✅ Analytics: Solução gratuita/simples (abstração de eventos)

**v1.1**:
9. ✅ Dark mode: v1.1
10. ✅ Notificações Push: v1.1 (se solução gratuita)

**v1.5**:
11. ✅ Checkout/Billing (aluno→criador): v1.5
12. ✅ SEO/Discovery (marketplace apps): v1.5

**Delegado ao Time**:
13. ✅ Infra/DevOps: Time define (sugestão: Vercel/Netlify v1)
14. ✅ Escalabilidade: Time define arquitetura multi-app

---

### ❓ NOVAS QUESTÕES (Para Cliente)

Ver arquivo detalhado: `12_DUVIDAS_VALIDACAO/duvidas_para_cliente.md`

**Loja de Recompensas**:
- Quem configura produtos? (Platform Admin ou Creator?)
- Tipos de produtos/benefícios?
- Conversão XP → Moedas (1:1 ou fórmula custom?)
- Integração fulfillment (entrega física)?

**Aplicação sem Upload**:
- Como validar honestidade?
- Peer review? Criador aprova manualmente?
- Aplicação bloqueia progresso ou é opcional?

**i18n (Multi-idioma)**:
- Conteúdo do Creator é multi-idioma?
- Quem traduz? (criador ou automático?)
- Quais idiomas em v1? (PT-BR apenas ou PT-BR + EN?)

**Arquitetura Multi-App**:
- Apps compartilham banco de dados ou isolados?
- Subdomínio automático (app123.plataforma.com)?
- Creator pode conectar domínio próprio em qual plano?

**Analytics/Notificações**:
- Dashboard custom no Creator Studio ou link externo?
- Email notificações suficiente ou precisa push gratuito?

---

**Última Atualização**: 2025-12-26 (Após respostas do cliente)
