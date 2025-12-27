# ROADMAP DE VERSÕES

**Roadmap oficial do produto: v1 → v1.1 → v1.5**

Este documento define o que entra em cada versão com base nas respostas do cliente e decisões do conselho.

[fonte: Respostas Cliente #1-15 → Definição de roadmap por versão]

---

## v1: CORE LEARNING + CREATOR STUDIO + LOJA RECOMPENSAS

### Objetivo
Produto mínimo viável com diferenciação científica completa. Permite criadores construírem InfoApps de aprendizagem ativa e alunos consumirem com retenção.

### Features Incluídas

#### LEARNER APP (Aluno)
- ✅ **Formatos Oficiais**: Missão, Aula Interativa (beats + checkpoints), Review SRS, Aplicação (sem upload)
- ✅ **Player Unificado**: PlayerFrame com CheckpointModule, progresso de beat, som/legendas
- ✅ **Aplicação Simplificada**: Checklist auto-declarativo + texto/link (SEM upload de arquivos)
- ✅ **Economia de Aprendizagem**: XP, badges, streak, ligas TEAM/PRIVATE
- ✅ **Loja de Recompensas**: Aluno troca moedas/XP por produtos/benefícios
- ✅ **Progresso/Trilha**: Mapa de progresso, histórico, badges
- ✅ **Onboarding**: 4 telas (objetivo, nível, rotina, preferências)
- ✅ **Configurações**: Som/haptics/motion (toggles de acessibilidade)
- ✅ **Notificações Email**: Streak em risco, novas missões (SEM push)

#### CREATOR STUDIO
- ✅ **Wizard Create InfoApp**: 4-5 passos (nome, nicho, branding, objetivos)
- ✅ **Import Pack**: Upload .zip estruturado (manifest.yaml + tracks.yaml + lessons/*.yaml)
- ✅ **Build por Formato**: Aula Interativa, Story, Missão, Review SRS, Aplicação
- ✅ **QA Checklist**: 6 gates automáticos bloqueiam publicação
- ✅ **Preview Emulator**: Simula telas do aluno (por perfil SAFE/TENSION/STATUS)
- ✅ **Publish + Versioning**: Publicação com changelog, rollback
- ✅ **Analytics Básico**: Funil de conclusão, drop-off por beat (dashboard simples)
- ✅ **Rewards/Economy**: Configurar XP, badges, loja
- ✅ **Settings**: Domínio/SEO, TTS (ElevenLabs), branding
- ✅ **Billing**: Planos (MicroSaaS/Standard/Full), quotas TTS/storage
- ✅ **Roles & Audit**: Permissões (owner/editor/viewer), logs

#### PLATFORM ADMIN
- ✅ **Tenants/Workspaces**: Gestão global de workspaces, suporte
- ✅ **Moderação**: Denúncias, conteúdo suspeito, bloqueios
- ✅ **Loja de Recompensas (Config)**: Catálogo global de produtos
- ✅ **Infra/Status**: Saúde do sistema, alertas de custo (TTS, storage)
- ✅ **Pagamentos**: Planos, cobrança, chargebacks
- ✅ **Auditoria**: Logs globais, incidentes

#### INFRAESTRUTURA/TÉCNICO
- ✅ **PWA Web Responsivo**: NÃO app native, installable (manifest.json + service worker)
- ✅ **TTS Cache Inteligente**: Gera áudio UMA vez, serve cached (cache global)
- ✅ **i18n Infraestrutura**: react-i18next ou next-intl, idioma inicial PT-BR
- ✅ **Analytics Gratuito**: Google Analytics 4, Plausible ou PostHog (abstração de eventos)
- ✅ **LGPD/GDPR**: Consentimentos obrigatórios, política de privacidade, exclusão de dados
- ✅ **Acessibilidade WCAG AA**: Contrast ratio, focus visível, ARIA labels, toggles reduce-motion/sound
- ✅ **Design System**: Tokens (spacing/type/color), componentes (35+), variantes SAFE/TENSION/STATUS
- ✅ **Sistema de Som**: Sound Kit v1, mapa por estado/ação, mix de volume
- ✅ **Multi-tenancy**: 1 workspace = 1 marca/criador, apps isolados

#### EXCLUÍDO DE v1
- ❌ Dark mode → v1.1
- ❌ Notificações Push → v1.1
- ❌ Offline mode → NÃO precisa
- ❌ Upload de arquivos em Aplicação → Removido
- ❌ Checkout/Billing (aluno→criador) → v1.5
- ❌ SEO/Discovery (marketplace apps) → v1.5
- ❌ App Native (iOS/Android) → v2+

[fonte: Respostas Cliente #1-15]

---

## v1.1: DARK MODE + NOTIFICAÇÕES PUSH (CONDICIONAL)

### Objetivo
Melhorias de UX e retenção sem aumentar escopo significativamente. Foco em features que melhoram experiência sem complexidade técnica alta.

### Features Incluídas

#### UX/ACESSIBILIDADE
- ✅ **Dark Mode**: Swap de cores via tokens, media query `prefers-color-scheme: dark`
- ✅ **Notificações Push**: OneSignal ou Firebase Cloud Messaging (APENAS se gratuito)
  - Tipos: Streak em risco, novas missões, unlock de badge
  - Opt-in (não opt-out)

#### ANALYTICS
- ✅ **Dashboard Analytics Avançado** (Creator Studio): Cohorts, heatmaps, filtros avançados
- ✅ **Auditoria de Acessibilidade Manual**: Teste com usuários reais, ajustes finos

#### OUTRAS MELHORIAS
- ✅ **i18n: Idiomas Adicionais**: EN, ES (tradução da interface)
- ✅ **Load Testing**: Teste de carga para validar escala 80+ apps

**CONDIÇÃO**: Dark mode e notificações push entram em v1.1 APENAS se não atrasarem entrega. Se houver risco de atraso, vão para v1.2.

[fonte: Respostas Cliente #4, #5 → Notificações e Dark mode em v1.1 se viável]

---

## v1.5: MONETIZAÇÃO + SEO/DISCOVERY

### Objetivo
Transformar plataforma em marketplace completo com billing e discovery. Permite criadores monetizarem apps e usuários descobrirem apps via SEO.

### Features Incluídas

#### BILLING/MONETIZAÇÃO
- ✅ **Checkout/Billing (Aluno → Criador)**: Stripe Connect
  - Aluno pode pagar por acesso a InfoApp
  - Criador recebe pagamento (menos fee da plataforma)
  - Múltiplas integrações: Stripe + MercadoPago/PagSeguro (Brasil)
  - PIX obrigatório (Brasil)

#### SEO/DISCOVERY
- ✅ **Marketplace Público de Apps**: Discovery centralizado de InfoApps
  - Featured apps (curadoria)
  - Search + filtros (nicho, rating, preço)
  - Categorias
- ✅ **SEO Avançado**: SSR/SSG (Next.js ou similar)
  - Landing pages SEO-friendly
  - Meta tags dinâmicas
  - Sitemap automático
- ✅ **Review Editorial**: Aprovação manual de apps para marketplace público

#### OUTRAS FEATURES
- ✅ **Domínio Personalizado**: Creator pode conectar domínio próprio (plano Full)
- ✅ **Fulfillment Integração**: Entrega de produtos físicos (merchandise)

**NOTA**: Cada produto criado tem funil próprio até v1.5. Discovery centralizado só entra em v1.5.

[fonte: Respostas Cliente #9, #10 → Checkout/Billing e SEO/Discovery em v1.5]

---

## v2+: NATIVE APPS + FUNCIONALIDADES AVANÇADAS

### Objetivo (Futuro)
Migração para apps nativos (iOS/Android) e funcionalidades avançadas baseadas em tração e feedback.

### Features Planejadas (A Confirmar)
- 🔮 **Apps Nativos**: React Native ou Flutter
- 🔮 **Offline Mode**: Cache de aulas/missões para uso offline
- 🔮 **AI/ML para Anti-Fraude**: Detecção automática de fraude em aplicações
- 🔮 **Gamificação Avançada**: Missões diárias customizadas, eventos temporários
- 🔮 **Social Features**: Grupos de estudo, peer review, fóruns
- 🔮 **White-label Completo**: Criador pode ter app completamente customizado (sem branding da plataforma)

**Decisões de v2+ dependem de**:
- Tração de v1/v1.1/v1.5
- Feedback de usuários/criadores
- Recursos disponíveis (time, orçamento)

---

## RESUMO: O QUE VAI EM CADA VERSÃO

| Feature | v1 | v1.1 | v1.5 | v2+ |
|---------|-----|------|------|-----|
| Formatos Oficiais (Missão/Aula/Aplicação/SRS) | ✅ | - | - | - |
| Aplicação SEM upload | ✅ | - | - | - |
| Loja de Recompensas (aluno gasta moedas) | ✅ | - | - | - |
| Creator Studio + QA Gates | ✅ | - | - | - |
| PWA Web Responsivo | ✅ | - | - | - |
| TTS Cache Inteligente | ✅ | - | - | - |
| i18n Infraestrutura (PT-BR) | ✅ | - | - | - |
| Analytics Gratuito | ✅ | - | - | - |
| LGPD/GDPR | ✅ | - | - | - |
| Acessibilidade WCAG AA | ✅ | - | - | - |
| Dark Mode | - | ✅ | - | - |
| Notificações Push (se grátis) | - | ✅ | - | - |
| i18n: EN/ES | - | ✅ | - | - |
| Dashboard Analytics Avançado | - | ✅ | - | - |
| Checkout/Billing (aluno→criador) | - | - | ✅ | - |
| SEO/Discovery (marketplace apps) | - | - | ✅ | - |
| Domínio Personalizado | - | - | ✅ | - |
| Fulfillment (produtos físicos) | - | - | ✅ | - |
| Apps Nativos (iOS/Android) | - | - | - | 🔮 |
| Offline Mode | - | - | - | 🔮 |
| AI/ML Anti-Fraude | - | - | - | 🔮 |

---

## CRITÉRIOS DE SUCESSO POR VERSÃO

### v1: Validação de Produto
- 🎯 **10+ criadores** criaram InfoApps
- 🎯 **500+ alunos** ativos (D7)
- 🎯 **Retenção D7 ≥ 40%**
- 🎯 **Completion rate ≥ 30%** (alunos que terminam aulas)
- 🎯 **QA Gates funcionam** (0 apps ruins publicados)

### v1.1: Crescimento e Otimização
- 🎯 **50+ criadores** criaram InfoApps
- 🎯 **2.000+ alunos** ativos (D7)
- 🎯 **Retenção D7 ≥ 50%** (com notificações push)
- 🎯 **Dark mode adotado por ≥30% dos usuários**

### v1.5: Monetização
- 🎯 **5+ criadores monetizando** (recebendo pagamento de alunos)
- 🎯 **GMV (Gross Merchandise Value) ≥ $10k/mês**
- 🎯 **SEO trazendo ≥20% do tráfego** (discovery orgânico)
- 🎯 **Marketplace público com ≥30 apps listados**

---

**Última Atualização**: 2025-12-26 (Após respostas do cliente)
