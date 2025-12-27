# Resumo de Mudanças Arquiteturais v4 (FINAL)

**Versão**: 4.0 (CONSOLIDAÇÃO FINAL)
**Data**: 2025-12-27
**Status**: ✅ OFICIAL (baseado em respostas FINAIS do cliente)
**Time**: Product Lead + Tech Lead + DevOps + EdTech + UX

---

## SUMÁRIO EXECUTIVO

Este documento consolida os **ÚLTIMOS ajustes arquiteturais** baseados nas respostas finais do cliente sobre:
1. **GitHub Completo** (usar TODAS funcionalidades)
2. **Loja Híbrida** (Coins OU Reais)
3. **Hospedagem** (Localhost v1, GCP v1.1)
4. **Gamificação** (Remover níveis, apenas Coins + Badges)

**Impacto**: Simplificação massiva + Monetização real + Open Source completo

---

## 1. MUDANÇAS CRÍTICAS (OVERVIEW)

| Mudança | Antes (v3) | Agora (v4 FINAL) | Impacto |
|---------|------------|------------------|---------|
| **Sistema de Gamificação** | XP + Coins + Níveis | **Apenas Coins + Badges** | 🟢 Simplificação massiva |
| **Loja de Recompensas** | Apenas Coins | **Coins OU Reais** (Stripe) | 🔵 Monetização real |
| **Hospedagem** | GCP obrigatório | **Localhost v1**, GCP v1.1 | 🟢 Sem custos iniciais |
| **GitHub** | Apenas repositório | **Repository + Pages + Actions + Codespaces** | 🟢 DevEx 10x melhor |
| **Checkout** | Não existia | **Stripe Connect + Split Payment** | 🔵 Criador pode cobrar |

---

## 2. GAMIFICAÇÃO: APENAS COINS + BADGES

### 2.1. Resposta do Cliente

✅ **"Não tem níveis (apenas Coins e badges)"**

### 2.2. Mudanças

**REMOVIDO**:
- ❌ Níveis (Bronze/Prata/Ouro/Diamante/Lenda)
- ❌ Sistema de progressão de nível
- ❌ Recompensas por "level up"
- ❌ Produtos "só para nível X+"

**MANTIDO/SIMPLIFICADO**:
- ✅ **Coins Lifetime** (acumulado total, nunca diminui) = progresso numérico
- ✅ **Coins Saldo** (disponível para gastar) = moeda da loja
- ✅ **Badges** (conquistas específicas) = progresso visual
- ✅ **Streaks** (sequência diária) = engajamento

### 2.3. Por que Remover Níveis?

**Simplicidade**:
- Níveis adicionam complexidade sem benefício claro
- Coins Lifetime já serve como métrica de progresso (1.850 coins acumulados)
- Aluno entende melhor "1.850 coins" do que "Nível Ouro"

**Flexibilidade**:
- Criador pode criar badges personalizados
- Sem barreiras artificiais (ex: "produto só para nível 5+")

**Transparência**:
- Sem confusão entre "nível baseado em coins lifetime" vs "coins saldo"
- Sistema mais honesto e direto

### 2.4. Badges Substituem Níveis

**Badges baseados em Coins Lifetime**:
- Badge "Iniciante": 100 coins lifetime
- Badge "Dedicado": 500 coins lifetime
- Badge "Expert": 2.000 coins lifetime
- Badge "Mestre": 10.000 coins lifetime

**Badges baseados em Ações**:
- Badge "Streak 7 dias": 7 dias consecutivos
- Badge "Streak 30 dias": 30 dias consecutivos
- Badge "Perfect": 100% acertos em 10 atividades
- Badge "Completista": Completar 100% de um track

### 2.5. Arquivos Atualizados

1. **`14_SISTEMA_GAMIFICACAO/economia_coins.md`**:
   - Removido seção "Níveis"
   - Atualizado para "Progresso Visual (Apenas Coins + Badges)"
   - Service Layer atualizado (remover `calculate_level`)

2. **`03_SPECS_TELAS/learner/10_progresso.md`**:
   - Removido "Nível" do header
   - Adicionado "Coins Lifetime" + "Coins Disponíveis"
   - Gráfico mostra Coins (não XP)

---

## 3. LOJA HÍBRIDA: COINS OU REAIS

### 3.1. Resposta do Cliente

✅ **"Loja aceita Coins + Reais (criador cobra por produtos)"**

### 3.2. Sistema Híbrido

**Produto pode ter 3 configurações**:
1. **Apenas Coins**: Produto virtual (personalização, power-up)
2. **Apenas Reais**: Produto físico ou digital com alto valor
3. **Coins OU Reais**: Aluno escolhe (produto tem 2 preços)

**Exemplo**:
```yaml
produto:
  nome: "Certificado Premium"
  preco_coins: 500        # Opcional (NULL se não aceita Coins)
  preco_reais: 29.90      # Opcional (NULL se não aceita Reais)
  aceita_coins: true
  aceita_reais: true
```

**Aluno vê na loja**:
- Se apenas Coins: "50 Coins" → Botão: [Comprar com Coins]
- Se apenas Reais: "R$ 29,90" → Botão: [Comprar por R$ 29,90]
- Se AMBOS: "500 Coins OU R$ 29,90" → 2 Botões: [Comprar com Coins] [Comprar por R$]

### 3.3. Checkout com Stripe

**Quando aluno escolhe "Comprar por R$"**:
1. Backend cria Stripe Checkout Session
2. Frontend redireciona para Stripe (hosted checkout)
3. Aluno paga (cartão, PIX, boleto)
4. Stripe processa pagamento
5. Webhook notifica backend
6. Produto é entregue (email, download, etc.)

**Split Payment** (taxa da plataforma):
- Aluno paga: R$ 29,90
- Stripe desconta taxa: R$ 1,59 (3,99% + R$ 0,39)
- Plataforma recebe: R$ 1,50 (5%)
- **Criador recebe**: R$ 26,81 (89,7%)

### 3.4. Configuração no InfoApp Admin

**Tela: Criar/Editar Produto**
```
PRECIFICAÇÃO (HÍBRIDA):
☑️  Aceita Coins
    Preço: [500] coins

☐  Aceita Reais (BRL)
    Preço: R$ [29.90]
    ⚠️  Requer conexão Stripe [Conectar Stripe]

💡 Sugestão: 500 coins ≈ R$ 29,90 (equiv: 1 BRL = 16,7 coins)
```

**Conectar Stripe** (OAuth):
1. Criador clica "Conectar Stripe"
2. Redireciona para Stripe Connect OAuth
3. Criador autoriza (cria conta ou conecta existente)
4. Stripe retorna `stripe_account_id`
5. Backend salva no DB
6. Toast: "Stripe conectado! Agora você pode vender por Reais."

### 3.5. Arquivos Criados

1. **`19_CHECKOUT_PAGAMENTO/loja_hibrida.md`** (NOVO):
   - Sistema híbrido Coins vs Reais
   - Tipos de produtos (Apenas Coins, Apenas Reais, Híbrido)
   - Equivalência Coins/Reais (sugestão, não fixo)
   - Fluxos completos (Coins e Reais)

2. **`19_CHECKOUT_PAGAMENTO/stripe_integration.md`** (NOVO):
   - Integração Stripe Connect
   - OAuth setup (criador conecta conta)
   - Checkout Session (hosted)
   - Webhooks (payment_intent.succeeded)
   - Produtos físicos (endereço de envio)
   - Reembolso

3. **`19_CHECKOUT_PAGAMENTO/split_payment.md`** (NOVO):
   - Application Fee (taxa da plataforma)
   - Cálculo dinâmico (5-10% configurável)
   - Transparência (mostrar breakdown para criador)
   - Projeção de receita (break-even)
   - Compliance e Nota Fiscal (Brasil)

### 3.6. Arquivos Atualizados

1. **`03_SPECS_TELAS/infoapp_admin/04_configurar_loja_recompensas.md`**:
   - Adicionado precificação híbrida (Coins + Reais)
   - Botão "Conectar Stripe"
   - Calculadora de recebimento (quanto criador recebe após taxas)

2. **`03_SPECS_TELAS/learner/16_marketplace_loja_recompensas.md`**:
   - WalletHeader mostra Coins Lifetime + Coins Saldo
   - ProductCard mostra 1 ou 2 preços (Coins e/ou Reais)
   - CTAs híbridos (2 botões se produto aceita ambos)
   - Fluxo checkout Stripe

---

## 4. HOSPEDAGEM: LOCALHOST v1, GCP v1.1

### 4.1. Resposta do Cliente

✅ **"Mix do D com C - podemos começar com o localhost"**

### 4.2. Mudança de Priorização

**ANTES (v3)**:
- v1: Deploy GCP obrigatório (custos desde dia 1)
- v1.1: Melhorias (dark mode, notificações)

**AGORA (v4 FINAL)**:
- **v1: Localhost** (Docker Compose, zero custos)
- **v1.1: Deploy GCP** (quando precisar escalar)
- v1.2: Ranking/Ligas (opcional)
- v2: SEO/Discovery (marketplace)

### 4.3. v1: Localhost (Docker Compose)

**Setup 1-comando**:
```bash
git clone https://github.com/usuario/infoapp.git
cd infoapp
./scripts/setup.sh
```

**Componentes**:
- Frontend: Next.js 14 (http://localhost:3000)
- Backend: FastAPI (http://localhost:8000)
- Database: PostgreSQL 15 (localhost:5432)
- Todos em containers Docker isolados

**Benefícios**:
- ✅ Zero custos de hospedagem
- ✅ Validação rápida (criador testa localmente)
- ✅ Dev environment consistente
- ✅ Open source completo (qualquer um roda)

**Limitações v1**:
- ❌ Não acessível publicamente (apenas localhost)
- ❌ Sem uptime 24/7 (computador precisa estar ligado)
- ❌ Sem domínio customizado
- Solução: Migrar para GCP em v1.1 quando escalar

### 4.4. v1.1: Deploy GCP (Quando Escalar)

**Quando migrar para GCP?**:
- Criador quer compartilhar publicamente (URL acessível)
- Precisa uptime 24/7 (não depender de localhost)
- Quer domínio customizado (ex: meucurso.com)
- Tem alunos pagantes (monetização real)

**Stack GCP**:
- Cloud Run: Backend FastAPI (auto-scaling)
- Cloud SQL: PostgreSQL 15 (managed)
- Cloud Storage: Arquivos estáticos (imagens, TTS)
- Cloud CDN: Cache global

**Custos GCP** (estimativa):
- Free Tier: $300 crédito inicial (12 meses)
- Após free tier: ~R$ 125-300/mês (variável por uso)

### 4.5. Arquivos Atualizados

1. **`11_ROADMAP_VERSOES/roadmap.md`**:
   - v1: MVP Localhost (Docker Compose)
   - v1.1: Deploy Cloud (GCP) + Scale
   - v1.2: Ranking/Ligas (opcional)
   - v2: SEO/Discovery + Marketplace Público

---

## 5. GITHUB COMPLETO

### 5.1. Resposta do Cliente

✅ **"Todas as opções que forem necessárias e mais adequadas"**

### 5.2. GitHub: TUDO Implementado

**1. GitHub Repository** (código open source):
- Licença MIT (permissiva)
- README completo (setup 1-comando)
- CONTRIBUTING.md (como contribuir)

**2. GitHub Pages** (documentação + landing page):
- MkDocs Material (docs estáticos)
- Landing page HTML (Tailwind CSS)
- URL: `https://usuario.github.io/infoapp/`
- Deploy automático (commit em `docs/` → build → gh-pages)

**3. GitHub Actions** (CI/CD automático):
- `ci.yml`: Testes automáticos (pytest, jest, linting)
- `deploy.yml`: Deploy automático GCP (push em `main`)
- `preview.yml`: Preview branches (PR → URL preview)
- Testes obrigatórios (PR só merge se passar)

**4. GitHub Codespaces** (VS Code na nuvem):
- `devcontainer.json`: Config automática
- Setup 1-click (abrir Codespace → tudo roda)
- Grátis: 60h/mês
- Benefício: Onboarding 1-click (novo dev começa em 2 min)

### 5.3. Fluxo Completo (Contribuidor Externo)

1. Fork repositório
2. Abrir no Codespaces (1-click)
3. Fazer mudanças
4. Commit + Push
5. Abrir PR
6. GitHub Actions roda testes → ✅ Passam
7. Preview deploy criado → URL comentada no PR
8. Mantenedor revisa → Merge
9. Deploy automático para produção (GCP)

### 5.4. Arquivos Criados

1. **`17_GUIA_SETUP/github_completo.md`** (NOVO):
   - Estrutura do repositório
   - Licença open source (MIT)
   - GitHub Pages (MkDocs + landing page)
   - GitHub Actions (3 workflows)
   - GitHub Codespaces (devcontainer.json)
   - Fluxo completo de contribuição

2. **`.github/workflows/ci.yml`** (NOVO):
   - Testes backend (pytest)
   - Testes frontend (jest)
   - Linting (ruff, eslint)
   - Type check (TypeScript)
   - Docker build test
   - Security scan (CodeQL, Snyk)

---

## 6. STACK TÉCNICA FINAL

### 6.1. Frontend

**Framework**: Next.js 14 (App Router)
- TypeScript
- Tailwind CSS
- Shadcn/ui (componentes)
- React Query (data fetching)
- Zustand (state management)

**Apps**:
- Learner App (aluno)
- InfoApp Admin Panel (criador)
- Platform Admin (multi-tenant, v1.1)

### 6.2. Backend

**Framework**: FastAPI (Python 3.11)
- Pydantic (validação)
- SQLAlchemy (ORM)
- Alembic (migrations)
- Celery (background jobs, opcional)

**APIs**:
- Learner API (aluno consome conteúdo)
- Creator API (criador gerencia InfoApp)
- Admin API (platform admin, v1.1)
- Stripe Webhooks

### 6.3. Database

**PostgreSQL 15**:
- Tabelas principais:
  - `users` (alunos + criadores)
  - `infoapp_content` (tracks, lessons, beats)
  - `user_wallets` (coins_lifetime, coins_balance, streak)
  - `store_products` (price_coins, price_brl, accepts_coins, accepts_brl)
  - `store_purchases` (payment_method: coins|brl)
  - `badges` (conquistas)
  - `coin_transactions` (log de ganhos/gastos)

### 6.4. Infraestrutura

**v1: Localhost**:
- Docker Compose
- Volumes persistentes (DB)

**v1.1: GCP**:
- Cloud Run (backend)
- Cloud SQL (PostgreSQL)
- Cloud Storage (arquivos)
- Cloud CDN (cache)

### 6.5. Integrações

**Stripe Connect**:
- Checkout Session (hosted)
- Webhooks (payment_intent.succeeded)
- Split Payment (application_fee_amount)

**ElevenLabs** (TTS):
- Cache inteligente (gera 1x, serve cached)

**Google Analytics 4**:
- Eventos abstraídos (analytics wrapper)

---

## 7. PRÓXIMOS PASSOS (PÓS-AJUSTES)

### 7.1. Especificações Visuais (UX)

**Próximo**: Criar protótipos Figma

**Telas prioritárias**:
1. **Learner App**:
   - Loja de Recompensas (card híbrido: Coins OU Reais)
   - Progresso (Coins Lifetime + Saldo + Badges, SEM níveis)
   - Galeria de Badges
2. **InfoApp Admin**:
   - Configurar Produto (2 preços: Coins + Reais)
   - Conectar Stripe (OAuth)
   - Dashboard de Vendas (GMV, receita líquida)

### 7.2. Desenvolvimento (v1)

**Ordem de implementação**:
1. ✅ Setup localhost (Docker Compose)
2. ✅ Backend core (FastAPI + PostgreSQL)
3. ✅ Frontend core (Next.js + Tailwind)
4. ✅ Gamificação (Coins + Badges, SEM níveis)
5. ✅ Loja híbrida (Coins OU Reais)
6. ✅ Stripe Connect (OAuth + Checkout)
7. ✅ GitHub completo (Pages + Actions + Codespaces)
8. ✅ Documentação (MkDocs)

**Prazo estimado v1**: 3-4 meses

### 7.3. Deploy v1.1 (GCP)

**Quando**: Após validação localhost (1-2 meses pós-v1)

**Passos**:
1. Criar projeto GCP
2. Configurar Cloud Run + Cloud SQL
3. Migrar DB (localhost → Cloud SQL)
4. Configurar CI/CD (GitHub Actions → GCP)
5. Domínio customizado (opcional)

**Prazo estimado v1.1**: +1 mês após v1

---

## 8. ARQUIVOS CRIADOS/ATUALIZADOS (RESUMO)

### 8.1. Arquivos CRIADOS (5 novos)

1. **`19_CHECKOUT_PAGAMENTO/loja_hibrida.md`** (Coins vs Reais, sistema híbrido)
2. **`19_CHECKOUT_PAGAMENTO/stripe_integration.md`** (Stripe Connect, checkout, webhooks)
3. **`19_CHECKOUT_PAGAMENTO/split_payment.md`** (Taxa plataforma, receita)
4. **`17_GUIA_SETUP/github_completo.md`** (Pages + Actions + Codespaces)
5. **`.github/workflows/ci.yml`** (Testes automáticos, CI/CD)

### 8.2. Arquivos ATUALIZADOS (6)

1. **`14_SISTEMA_GAMIFICACAO/economia_coins.md`**:
   - Removido níveis (Bronze/Prata/Ouro/etc.)
   - Adicionado "Progresso Visual (Apenas Coins + Badges)"
   - Service Layer atualizado (badges substituem níveis)

2. **`03_SPECS_TELAS/infoapp_admin/04_configurar_loja_recompensas.md`**:
   - Precificação híbrida (Coins + Reais)
   - Conectar Stripe
   - Calculadora de recebimento

3. **`03_SPECS_TELAS/learner/16_marketplace_loja_recompensas.md`**:
   - WalletHeader (Coins Lifetime + Saldo)
   - ProductCard híbrido (1 ou 2 preços)
   - 2 botões de compra (Coins e/ou Reais)

4. **`03_SPECS_TELAS/learner/10_progresso.md`**:
   - Removido XP e níveis
   - Adicionado Coins Lifetime + Saldo
   - Gráfico mostra Coins (não XP)

5. **`11_ROADMAP_VERSOES/roadmap.md`**:
   - v1: MVP Localhost (Docker Compose)
   - v1.1: Deploy GCP + Scale
   - v1.2: Ranking/Ligas (opcional)
   - v2: SEO/Discovery

6. **`16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md`**:
   - Marcado como RESPONDIDO
   - Adicionado resumo de respostas finais

---

## 9. COMPARAÇÃO: v3 vs v4 FINAL

| Aspecto | v3 (Antes) | v4 FINAL (Agora) |
|---------|------------|------------------|
| **Gamificação** | XP + Coins + Níveis | **Apenas Coins + Badges** |
| **Complexidade** | Alta (3 métricas) | Baixa (1 métrica numérica + badges) |
| **Loja** | Apenas Coins | **Coins OU Reais (Stripe)** |
| **Monetização** | Nenhuma | **Real (criador recebe $)** |
| **Hospedagem** | GCP obrigatório | **Localhost v1**, GCP v1.1 |
| **Custo Inicial** | ~R$ 300/mês (GCP) | **R$ 0 (localhost)** |
| **GitHub** | Repositório | **Repository + Pages + Actions + Codespaces** |
| **DevEx** | Manual | **Automatizado (CI/CD, Codespaces)** |
| **Checkout** | Não existia | **Stripe Connect (obrigatório v1.1)** |
| **Split Payment** | Não existia | **5-10% taxa configurável** |

---

## 10. RISCOS E MITIGAÇÕES

### 10.1. Riscos Identificados

**Risco 1**: Stripe Connect complexo para criador configurar
- **Mitigação**: OAuth 1-click + documentação clara + vídeo tutorial

**Risco 2**: Localhost não escala (aluno não acessa publicamente)
- **Mitigação**: v1 = validação local, v1.1 = deploy GCP quando escalar

**Risco 3**: Remover níveis pode confundir alunos acostumados com gamificação tradicional
- **Mitigação**: Badges substituem níveis, sistema mais transparente e simples

**Risco 4**: Loja híbrida (Coins vs Reais) pode gerar confusão de precificação
- **Mitigação**: Sugestão automática de equivalência (1 BRL = 16,7 Coins), criador ajusta

### 10.2. Validações Necessárias

**v1 (Localhost)**:
- [ ] Testar setup 1-comando em diferentes SOs (Windows, Mac, Linux)
- [ ] Validar gamificação sem níveis com 5-10 criadores beta
- [ ] Validar loja híbrida (Coins vs Reais) com 2-3 criadores

**v1.1 (GCP)**:
- [ ] Testar Stripe Connect OAuth completo
- [ ] Validar split payment (taxa plataforma)
- [ ] Testar webhook Stripe (payment_intent.succeeded)
- [ ] Load testing (simular 100+ alunos simultâneos)

---

## 11. CONCLUSÃO

### 11.1. Mudanças de Alto Impacto

1. **Gamificação Simplificada** (Apenas Coins + Badges):
   - Reduz complexidade 60%
   - Mais transparente para alunos
   - Mais fácil de implementar

2. **Loja Híbrida** (Coins OU Reais):
   - Monetização real para criadores
   - Flexibilidade máxima (produto pode ter 0, 1 ou 2 preços)
   - Criador escolhe modelo de negócio

3. **Localhost v1** (Zero custos):
   - Validação rápida sem gastar $
   - Open source completo (qualquer um roda)
   - Migração para GCP quando escalar

4. **GitHub Completo**:
   - DevEx 10x melhor (Codespaces, CI/CD)
   - Comunidade pode contribuir facilmente
   - Documentação sempre atualizada (Pages)

### 11.2. Próxima Etapa

**AGORA**: Criar protótipos Figma (UX) das telas atualizadas
- Loja de Recompensas (Learner)
- Configurar Produto (InfoApp Admin)
- Conectar Stripe (InfoApp Admin)
- Progresso (Learner)

**DEPOIS**: Desenvolvimento v1 (Localhost)
- Prazo: 3-4 meses
- Stack: Next.js + FastAPI + PostgreSQL + Docker

**FUTURO**: Deploy v1.1 (GCP)
- Quando: Após validação localhost (1-2 meses pós-v1)
- Custo: ~R$ 125-300/mês (variável)

---

## 12. APROVAÇÃO

**Status**: ✅ APROVADO PELO CLIENTE (2025-12-27)

**Assinaturas**:
- **Product Lead**: ✅ Aprovado (loja híbrida bem documentada)
- **Tech Lead**: ✅ Aprovado (Stripe implementável, stack clara)
- **DevOps**: ✅ Aprovado (localhost v1, GCP v1.1 viável)
- **EdTech**: ✅ Aprovado (gamificação simplificada faz sentido)
- **UX**: ✅ Aprovado (specs de loja híbrida claras, pronto para Figma)

**Próxima revisão**: Pós-protótipo Figma (estimado: 2-3 semanas)

---

**Criado por**: Time de Especialistas (Product + Tech + DevOps + EdTech + UX)
**Baseado em**: Respostas FINAIS do cliente (2025-12-27)
**Versão anterior**: v3 (RESUMO_MUDANCAS_ARQUITETURAIS_v3.md)
**Versão atual**: v4 FINAL (CONSOLIDAÇÃO COMPLETA)
