# ROADMAP DE VERSÕES

**Roadmap oficial do produto: v1 → v1.1 → v1.5**

Este documento define o que entra em cada versão com base nas respostas do cliente e decisões do conselho.

[fonte: Respostas Cliente #1-15 → Definição de roadmap por versão]

---

## v1: MVP LOCALHOST (Docker Compose)

### Objetivo
Produto mínimo viável rodando **localhost** (Docker Compose). Foco em validação rápida sem custos de hospedagem. Criadores testam localmente antes de decidir deploy cloud.

### Features Incluídas

#### LEARNER APP (Aluno)
- ✅ **Formatos Oficiais**: Missão, Aula Interativa (beats + checkpoints), Review SRS, Aplicação (sem upload)
- ✅ **Player Unificado**: PlayerFrame com CheckpointModule, progresso de beat, som/legendas
- ✅ **Aplicação Simplificada**: Checklist auto-declarativo + texto/link (SEM upload de arquivos)
- ✅ **Gamificação Simplificada**: **Apenas Coins + Badges + Streaks** (SEM XP, SEM Níveis)
- ✅ **Loja Híbrida**: Produtos com Coins OU Reais (Stripe Connect opcional)
- ✅ **Progresso/Trilha**: Coins lifetime/disponíveis, histórico, badges
- ✅ **Onboarding**: 4 telas (objetivo, rotina, preferências)
- ✅ **Configurações**: Som/haptics/motion (toggles de acessibilidade)
- ✅ **Notificações Email**: Streak em risco, novas missões (SEM push)

#### INFOAPP ADMIN PANEL (Criador)
- ✅ **Wizard Create InfoApp**: 4-5 passos (nome, nicho, branding, objetivos)
- ✅ **Import Pack**: Upload YAML/JSON estruturado (manifest + tracks + lessons)
- ✅ **Build por Formato**: Aula Interativa, Story, Missão, Review SRS, Aplicação
- ✅ **QA Checklist**: 6 gates automáticos bloqueiam publicação
- ✅ **Preview Emulator**: Simula telas do aluno (por perfil SAFE/TENSION/STATUS)
- ✅ **Publish + Versioning**: Publicação com changelog, rollback
- ✅ **Analytics Básico**: Funil de conclusão, drop-off por beat (dashboard simples)
- ✅ **Loja Híbrida**: Configurar produtos (Coins + Reais), conectar Stripe
- ✅ **Settings**: TTS (ElevenLabs), branding, multi-idioma
- ✅ **Roles & Audit**: Permissões (owner/editor/viewer), logs

#### PLATFORM ADMIN (v1.1)
- ⚠️ **MOVIDO PARA v1.1**: Platform Admin não necessário para localhost
- Localhost = 1 criador, 1 InfoApp (sem multi-tenancy)

#### INFRAESTRUTURA/TÉCNICO
- ✅ **Localhost Docker Compose**: Setup 1-comando (`./scripts/setup.sh`)
  - Frontend (Next.js 14)
  - Backend (FastAPI Python 3.11)
  - Database (PostgreSQL 15)
  - Todos rodando em containers isolados
- ✅ **PWA Web Responsivo**: NÃO app native, installable (manifest.json + service worker)
- ✅ **TTS Cache Inteligente**: Gera áudio UMA vez, serve cached
- ✅ **Multi-idioma**: PT-BR, EN-US, ES-ES (i18next)
- ✅ **Analytics Gratuito**: Google Analytics 4 (abstração de eventos)
- ✅ **LGPD/GDPR**: Consentimentos obrigatórios
- ✅ **Acessibilidade WCAG AA**: Contrast ratio, focus visível, ARIA labels
- ✅ **Design System**: Tokens (spacing/type/color), componentes (35+)
- ✅ **Sistema de Som**: Sound Kit v1, mapa por estado/ação
- ✅ **GitHub Completo**: Repository + Pages (docs) + Actions (CI/CD) + Codespaces
- ✅ **Stripe Connect**: Pagamentos produtos em Reais (opcional)

#### EXCLUÍDO DE v1
- ❌ Dark mode → v1.2
- ❌ Notificações Push → v1.2
- ❌ Offline mode → NÃO precisa
- ❌ Upload de arquivos em Aplicação → REMOVIDO permanentemente
- ❌ Deploy GCP/Cloud → v1.1
- ❌ Multi-tenancy/Platform Admin → v1.1
- ❌ SEO/Discovery (marketplace apps) → v2
- ❌ App Native (iOS/Android) → v2+
- ❌ Níveis/XP → REMOVIDO permanentemente (apenas Coins + Badges)

[fonte: Respostas Cliente FINAIS → Localhost v1, GCP v1.1]

---

## v1.1: DEPLOY CLOUD (GCP) + SCALE

### Objetivo
Migração de localhost para produção cloud (GCP). Foco em escala, uptime 24/7, e monetização real (Stripe obrigatório).

### Features Incluídas

#### DEPLOY E INFRAESTRUTURA
- ✅ **Deploy GCP (Cloud Run + Cloud SQL)**:
  - Cloud Run: Backend FastAPI (auto-scaling)
  - Cloud SQL: PostgreSQL 15 (managed)
  - Cloud Storage: Arquivos estáticos (imagens, áudios TTS)
  - Cloud CDN: Cache global
- ✅ **CI/CD Automático**: GitHub Actions → Deploy GCP
- ✅ **Domínio Customizado**: Criador conecta domínio próprio
- ✅ **SSL/HTTPS**: Certificados automáticos (Let's Encrypt)
- ✅ **Uptime 24/7**: Monitoramento (Cloud Monitoring)
- ✅ **Backups Automáticos**: Cloud SQL daily backups

#### MULTI-TENANCY E PLATFORM ADMIN
- ✅ **Platform Admin**: Gestão multi-tenant (múltiplos criadores)
- ✅ **Billing/Planos**: Criador paga pela plataforma (opcional)
- ✅ **Moderação**: Denúncias, conteúdo suspeito

#### MONETIZAÇÃO (STRIPE OBRIGATÓRIO)
- ✅ **Stripe Connect**: Criador recebe pagamentos de produtos em Reais
- ✅ **Split Payment**: Plataforma cobra taxa (5-10% configurável)
- ✅ **Produtos Físicos**: Integração transportadora (rastreamento)

#### MELHORIAS UX
- ✅ **Dark Mode**: Swap de cores via tokens
- ✅ **Notificações Push**: OneSignal ou Firebase (opcional)
- ✅ **Dashboard Analytics Avançado**: Cohorts, heatmaps

[fonte: Resposta Cliente FINAL → "Mix do D com C - podemos começar com localhost"]

---

## v1.2: RANKING/LIGAS (OPCIONAL)

### Objetivo
Adicionar competição social (ranking semanal, ligas) para criadores que queiram gamificação competitiva. Sistema 100% opcional.

### Features Incluídas
- ✅ **Ranking Semanal**: Alunos competem por Coins ganhos na semana
- ✅ **Ligas** (opcional): Sistema tipo Duolingo (Bronze → Diamante)
- ✅ **Recompensas Competitivas**: Top 3 ganham bonus coins + badges
- ✅ **Toggle On/Off**: Criador escolhe se quer ranking ou não

**Por que opcional?**:
- Foco em aprendizagem colaborativa (não competitiva)
- Contextos corporativos/acadêmicos preferem sem competição

---

## v2: SEO/DISCOVERY + MARKETPLACE PÚBLICO

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
