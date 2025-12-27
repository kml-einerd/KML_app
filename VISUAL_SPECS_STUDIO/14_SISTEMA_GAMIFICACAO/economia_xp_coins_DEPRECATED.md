# Sistema de Gamificação: Economia XP vs Coins

**Versão**: 2.0
**Data**: 2025-12-26
**Status**: OFICIAL
**Referência**: Sistema tipo Duolingo (múltiplas formas inteligentes de bonificar)

---

## 1. VISÃO GERAL

O sistema de gamificação tem **dois pilares independentes**:

1. **XP (Experience Points)**: Representa **progresso** e **maestria**
   - Sobe nível do aluno
   - Desbloqueia badges
   - Mostra evolução
   - **NÃO se gasta** (é acumulativo)

2. **Coins**: Moeda do InfoApp que **se gasta** na Loja de Recompensas
   - Compra produtos (desconto, físico, digital, personalização)
   - Incentiva engajamento contínuo
   - **Gasta e recarrega** (economia circular)

**Cliente disse**: "precisa ter um sistema de gamificação com múltiplas formas inteligentes de bonificar, tipo o Duolingo, mas que pode usar na lojinha"

---

## 2. XP (EXPERIENCE POINTS)

### 2.1. O que é XP?

XP representa o **progresso acumulado** do aluno no InfoApp.

**Características**:
- Acumulativo (nunca diminui)
- Sobe nível do aluno (Level 1, 2, 3...)
- Desbloqueia badges e conquistas
- Visível no perfil e ranking
- NÃO se gasta (é permanente)

### 2.2. Como ganhar XP?

| Ação | XP Base | XP com Bonus | Observações |
|------|---------|--------------|-------------|
| **Completar Beat** | 5 XP | 5-10 XP | +5 XP se acerto perfeito |
| **Completar Checkpoint** | 10 XP | 10-20 XP | +10 XP se acerto na 1ª tentativa |
| **Completar Aula Interativa** | 50 XP | 50-100 XP | +50 XP se 100% acertos |
| **Completar Missão Diária** | 30 XP | 30-60 XP | +30 XP se completar streak 3+ dias |
| **Completar Atividade Interativa** | 80 XP | 80-160 XP | +80 XP se perfect score |
| **Completar Review SRS** | 20 XP | 20-40 XP | +20 XP se 100% recall |
| **Manter Streak 7 dias** | 100 XP | 100 XP | Bonus semanal |
| **Subir de Nível** | 0 XP | 0 XP | Consequência de XP acumulado |
| **Ganhar Badge** | 50 XP | 50 XP | Bonus por conquista |
| **Completar Estação** | 200 XP | 200-400 XP | +200 XP se 100% conclusão |

### 2.3. Sistema de Níveis

**Fórmula**: XP necessário para próximo nível = `100 * level^1.5`

| Nível | XP Total Necessário | XP para Próximo Nível |
|-------|---------------------|----------------------|
| 1 → 2 | 100 | 100 |
| 2 → 3 | 282 | 182 |
| 3 → 4 | 519 | 237 |
| 4 → 5 | 800 | 281 |
| 5 → 6 | 1.136 | 336 |
| 10 → 11 | 3.162 | 525 |
| 20 → 21 | 8.944 | 880 |
| 50 → 51 | 35.355 | 1.757 |

**Visual no Learner App**:
```
┌─────────────────────────────────────────────┐
│  Nível 5                        [1.136/1.472 XP] │
│  ███████████████░░░░░░░░░░░░░░░ 77%        │
└─────────────────────────────────────────────┘
```

### 2.4. Badges desbloqueados por XP

| Badge | XP Necessário | Descrição |
|-------|---------------|-----------|
| 🌱 Iniciante | 0 XP | Primeiro login |
| 🔥 Aprendiz | 500 XP | Nível 3 |
| ⭐ Dedicado | 1.500 XP | Nível 7 |
| 💎 Mestre | 5.000 XP | Nível 15 |
| 👑 Lenda | 20.000 XP | Nível 30 |

---

## 3. COINS (MOEDA DO INFOAPP)

### 3.1. O que são Coins?

Coins são a **moeda virtual** que alunos gastam na **Loja de Recompensas** do InfoApp.

**Características**:
- Moeda que se gasta (não é acumulativa como XP)
- Ganha por ações de aprendizado
- Gasta em produtos da loja (desconto, físico, digital, personalização)
- **Incentiva engajamento contínuo** (precisa ganhar mais para comprar)
- Visível no perfil e no header do app

### 3.2. Como ganhar Coins?

**Cliente disse**: "multiplas formas inteligentes de bonificar, tipo o Duolingo"

| Ação | Coins Base | Coins com Bonus | Observações |
|------|------------|-----------------|-------------|
| **Completar Beat** | 1 coin | 1-2 coins | +1 coin se acerto perfeito |
| **Checkpoint correto (1ª tentativa)** | 2 coins | 2-5 coins | +3 coins se streak de 5+ acertos |
| **Completar Aula Interativa** | 10 coins | 10-20 coins | +10 coins se 100% acertos |
| **Completar Missão Diária** | 5 coins | 5-15 coins | +10 coins se completar antes das 12h |
| **Completar Atividade Interativa** | 15 coins | 15-30 coins | +15 coins se perfect score |
| **Completar Review SRS** | 5 coins | 5-10 coins | +5 coins se 100% recall |
| **Manter Streak (por dia)** | 3 coins | 3-10 coins | +7 coins se streak 7+ dias |
| **Streak de 7 dias** | 50 coins | 50 coins | Bonus semanal |
| **Streak de 30 dias** | 200 coins | 200 coins | Bonus mensal |
| **Completar Daily Goal** | 10 coins | 10 coins | Meta diária (ex: 50 XP/dia) |
| **Subir de Nível** | 20 coins | 20-100 coins | Escalona: Nível 10 = 100 coins |
| **Ganhar Badge** | 25 coins | 25 coins | Bonus por conquista |
| **Completar Estação** | 100 coins | 100-200 coins | +100 coins se 100% conclusão |
| **Convidar amigo (referral)** | 50 coins | 50 coins | Quando amigo completa 1ª lição |
| **Responder pesquisa NPS** | 10 coins | 10 coins | Feedback para criador |

### 3.3. Sistema de Streaks (Chave para Coins)

**Streak**: Dias consecutivos que aluno completa Daily Goal

**Daily Goal**: Meta configurável (padrão: 50 XP/dia ou 1 lição/dia)

**Bonificação por Streak**:
| Streak | Bonus Coins Diário | Bonus Milestone |
|--------|-------------------|-----------------|
| 1 dia | +3 coins | - |
| 3 dias | +5 coins | +10 coins |
| 7 dias | +7 coins | +50 coins |
| 14 dias | +10 coins | +100 coins |
| 30 dias | +15 coins | +200 coins |
| 100 dias | +20 coins | +500 coins |

**Visual no Learner App**:
```
┌─────────────────────────────────────────────┐
│  🔥 Streak: 7 dias                          │
│  ███████░░░░░░░░░░░░░░░░░░░░░░              │
│  Próximo milestone: 14 dias (+100 coins)    │
└─────────────────────────────────────────────┘
```

### 3.4. Perfect Score Bonus

**Perfect Score**: Completar atividade com 100% de acertos na 1ª tentativa

**Bonificação**:
- Aula Interativa: +10 coins
- Atividade Interativa: +15 coins
- Review SRS: +5 coins

**Visual**:
```
┌─────────────────────────────────────────────┐
│  ✨ PERFECT SCORE!                          │
│  🎯 100% acertos na 1ª tentativa            │
│  +80 XP  +30 coins  (+15 bonus perfect!)    │
└─────────────────────────────────────────────┘
```

### 3.5. Time Bonus (Opcional v1.1)

**Time Bonus**: Completar lição/atividade mais rápido que tempo médio

**Bonificação**:
- 50% mais rápido: +5 coins
- 75% mais rápido: +10 coins

**Exemplo**: Tempo médio da lição = 10min. Aluno completa em 5min → +5 coins

### 3.6. Combo Multiplier (Opcional v1.1)

**Combo**: Acertos consecutivos em checkpoints

**Multiplicador**:
- 5 acertos seguidos: 1.5x coins
- 10 acertos seguidos: 2x coins
- 20 acertos seguidos: 3x coins

**Visual**:
```
┌─────────────────────────────────────────────┐
│  🔥 COMBO x2!                               │
│  10 acertos consecutivos                    │
│  Próximo checkpoint vale 4 coins!           │
└─────────────────────────────────────────────┘
```

---

## 4. CONVERSÃO XP ↔ COINS?

### 4.1. Decisão: NÃO CONVERTER

**XP e Coins são sistemas independentes**:
- XP = Progresso (acumula, não gasta)
- Coins = Moeda (gasta, recarrega)

**Razão**: Se converter XP → Coins, aluno pode "gastar progresso" (ruim para UX)

**Alternativa**: Aluno ganha **ambos** ao completar atividades (veja tabelas acima)

### 4.2. Exemplo de ganhos simultâneos

**Aluno completa Aula Interativa com 100% acertos**:
- +100 XP (50 base + 50 bonus perfect)
- +20 Coins (10 base + 10 bonus perfect)
- +1 Badge (se for 1ª perfect score) → +50 XP + 25 Coins extras

**Total**: +150 XP + 45 Coins

---

## 5. LOJA DE RECOMPENSAS (COMO GASTAR COINS)

**Cliente disse**: "é configurado no admin do infoapp e não no modo studio e seria pelo usuario administrador"

### 5.1. Tipos de Produtos

| Tipo | Exemplos | Preço Sugerido |
|------|----------|----------------|
| **Desconto** | 10% off em produto do criador | 50-100 coins |
| **Produto Físico** | Camiseta, caneca, livro impresso | 500-1000 coins |
| **Produto Digital** | Ebook bonus, template, certificado premium | 100-300 coins |
| **Personalização** | Tema escuro, avatar premium, efeitos sonoros | 50-200 coins |
| **Power-ups** | Freeze streak (1 dia sem perder), XP boost 2x | 30-100 coins |
| **Conteúdo Exclusivo** | Módulo avançado, aula bônus | 200-500 coins |

### 5.2. Configuração pelo Criador (InfoApp Admin Panel)

**Tela: Configurar Loja de Recompensas**

Criador pode:
- CRUD de produtos (nome, descrição, imagem, preço em coins)
- Definir estoque (limitado/ilimitado)
- Ativar/desativar produtos
- Ver histórico de compras
- Definir regras (ex: produto só aparece para alunos nível 5+)

**Estética**: Amazon/Mercado Livre simplificada (card de produto com imagem, nome, preço, botão "Comprar")

### 5.3. UX no Learner App

**Tela: Loja de Recompensas** (ver spec: `03_SPECS_TELAS/learner/16_marketplace_loja_recompensas.md`)

```
┌─────────────────────────────────────────────┐
│  💰 Seus Coins: 245                         │
│  ─────────────────────────────────────────  │
│  [Card Produto 1]  [Card Produto 2]         │
│  Tema Escuro       Certificado Premium      │
│  50 coins          100 coins                │
│  [Comprar]         [Comprar]                │
└─────────────────────────────────────────────┘
```

**Fluxo de compra**:
1. Aluno clica "Comprar"
2. Modal: "Confirmar compra? -50 coins"
3. Confirma → Coins são debitados, produto é entregue
4. Notificação: "Tema Escuro ativado! 🎉"

---

## 6. ECONOMIA CIRCULAR: GANHAR vs GASTAR

### 6.1. Objetivo: Engajamento Contínuo

**Problema**: Se aluno acumula muitos coins e compra tudo, perde motivação
**Solução**: Economia balanceada com produtos novos sazonais

### 6.2. Tabela de Ganhos vs Gastos (Estimativa)

**Aluno médio (1 lição/dia, streak 7 dias)**:

**Ganhos semanais**:
- 7 lições: 70 XP + 70 coins
- 7 checkpoints corretos: 140 XP + 14 coins
- Streak 7 dias: 100 XP + 50 coins
- Daily goal (7x): 70 coins
- **Total**: ~310 XP + ~204 coins/semana

**Gastos típicos**:
- Tema Escuro: 50 coins (1x)
- Power-up Freeze: 30 coins (1-2x/mês)
- Desconto 10%: 100 coins (1x/mês)
- **Total**: ~180 coins/mês

**Balanço**: Aluno acumula ~600 coins/mês e gasta ~180 → sobra ~420 coins → pode comprar produto digital (300 coins)

### 6.3. Produtos Sazonais/Limitados

**Estratégia**: Criador adiciona produtos limitados para incentivar gasto

**Exemplos**:
- "Camiseta Edição Limitada" (800 coins, 50 unidades)
- "Acesso antecipado Módulo 5" (500 coins, disponível por 7 dias)
- "Avatar de Natal" (100 coins, disponível em dezembro)

---

## 7. LIGAS E RANKING (COMPETIÇÃO SOCIAL)

### 7.1. Ligas por XP

**Sistema**: Alunos competem semanalmente em ligas baseadas em XP ganho na semana

**Ligas** (tipo Duolingo):
1. Bronze (todos começam aqui)
2. Prata (top 10 de Bronze)
3. Ouro (top 10 de Prata)
4. Diamante (top 10 de Ouro)
5. Lenda (top 10 de Diamante)

**Recompensas**:
- Top 3 de cada liga: +50 coins
- Top 1: +100 coins + Badge

**Visual**:
```
┌─────────────────────────────────────────────┐
│  🏆 Liga Ouro - Semana 12/2025              │
│  ─────────────────────────────────────────  │
│  1. @joao        1.245 XP  ⬆️ Diamante       │
│  2. @maria       1.120 XP  ⬆️ Diamante       │
│  3. @pedro       1.050 XP  ⬆️ Diamante       │
│  ...                                        │
│  15. Você          580 XP  ➡️ Ouro           │
│  ...                                        │
│  48. @ana          120 XP  ⬇️ Prata          │
└─────────────────────────────────────────────┘
```

### 7.2. Ranking Global

**Ranking**: Top alunos por XP total (all-time)

**Recompensas**:
- Top 10 global: Badge especial
- Top 1 global: Badge "🏆 Número 1" + 500 coins

---

## 8. DAILY GOALS E PERSONALIZAÇÃO

### 8.1. Daily Goal Configurável

**Aluno escolhe meta diária**:
- Casual: 25 XP/dia (1 lição rápida)
- Regular: 50 XP/dia (1 lição completa)
- Sério: 100 XP/dia (2 lições)
- Intenso: 200 XP/dia (4+ lições)

**Recompensa**: Completar Daily Goal → +10 coins

### 8.2. Notificações Inteligentes

**Streak em risco**:
- "🔥 Seu streak de 15 dias vai expirar em 2h! Complete 1 lição rápida para manter."

**Daily goal incompleto**:
- "Faltam 20 XP para sua meta diária! 5 min de Review SRS e você consegue 🎯"

**Milestone próximo**:
- "Você está a 50 XP de subir para Nível 10! 🚀"

---

## 9. ANTI-CHEAT E LIMITES

### 9.1. Prevenção de Exploits

**Problema**: Aluno pode tentar "farmar" coins fazendo/refazendo lições infinitamente

**Soluções**:
1. **Limite de refazer**: Lição só dá coins na 1ª conclusão (refazer dá 50% XP, 0 coins)
2. **Cooldown**: Não pode refazer lição nas primeiras 24h
3. **Rate limit**: Máximo 10 lições/dia dão coins (além disso, só XP)
4. **Detecção de padrões**: Se aluno completa lição em < 10 seg, não dá coins

### 9.2. Sistema de Reports

**Admin pode**:
- Ver alunos com padrões suspeitos (ex: 100 lições/dia)
- Ajustar coins manualmente (débito/crédito)
- Banir aluno (se fraude confirmada)

---

## 10. IMPLEMENTAÇÃO TÉCNICA

### 10.1. Tabelas DB (InfoApp schema)

```sql
-- Wallet do aluno
CREATE TABLE user_wallets (
  user_id UUID PRIMARY KEY,
  xp_total INTEGER DEFAULT 0,
  coins_balance INTEGER DEFAULT 0,
  level INTEGER DEFAULT 1,
  streak_days INTEGER DEFAULT 0,
  last_activity_date DATE,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Histórico de transações XP
CREATE TABLE xp_transactions (
  id UUID PRIMARY KEY,
  user_id UUID,
  amount INTEGER,  -- pode ser negativo (ajuste manual)
  source VARCHAR(50),  -- 'lesson_complete', 'badge_earned', etc.
  metadata JSONB,  -- {'lesson_id': '...', 'perfect_score': true}
  created_at TIMESTAMP DEFAULT NOW()
);

-- Histórico de transações Coins
CREATE TABLE coin_transactions (
  id UUID PRIMARY KEY,
  user_id UUID,
  amount INTEGER,  -- positivo = ganho, negativo = gasto
  type VARCHAR(20),  -- 'earned', 'spent', 'adjusted'
  source VARCHAR(50),  -- 'lesson_complete', 'store_purchase', etc.
  metadata JSONB,  -- {'product_id': '...', 'price': 50}
  created_at TIMESTAMP DEFAULT NOW()
);

-- Produtos da loja
CREATE TABLE store_products (
  id UUID PRIMARY KEY,
  infoapp_id UUID,  -- tenant_id
  name VARCHAR(255),
  description TEXT,
  image_url VARCHAR(500),
  price_coins INTEGER,
  type VARCHAR(50),  -- 'discount', 'physical', 'digital', 'customization', 'powerup'
  stock_quantity INTEGER,  -- NULL = ilimitado
  active BOOLEAN DEFAULT true,
  metadata JSONB,  -- config específica por tipo
  created_at TIMESTAMP DEFAULT NOW()
);

-- Compras
CREATE TABLE store_purchases (
  id UUID PRIMARY KEY,
  user_id UUID,
  product_id UUID,
  price_paid INTEGER,  -- coins gastos
  status VARCHAR(20),  -- 'completed', 'pending', 'cancelled'
  delivered_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 10.2. Service Layer

```python
# Pseudocódigo
class GamificationService:
    def award_xp(user_id, amount, source, metadata=None):
        # Adiciona XP
        wallet = UserWallet.get(user_id)
        wallet.xp_total += amount

        # Verifica level up
        new_level = calculate_level(wallet.xp_total)
        if new_level > wallet.level:
            wallet.level = new_level
            award_coins(user_id, level_up_coins(new_level), 'level_up')

        # Log transação
        XPTransaction.create(user_id, amount, source, metadata)

        wallet.save()
        return wallet

    def award_coins(user_id, amount, source, metadata=None):
        # Adiciona coins
        wallet = UserWallet.get(user_id)
        wallet.coins_balance += amount

        # Log transação
        CoinTransaction.create(user_id, amount, 'earned', source, metadata)

        wallet.save()
        return wallet

    def spend_coins(user_id, product_id):
        # Compra produto
        product = StoreProduct.get(product_id)
        wallet = UserWallet.get(user_id)

        if wallet.coins_balance < product.price_coins:
            raise InsufficientCoins()

        # Debita coins
        wallet.coins_balance -= product.price_coins
        CoinTransaction.create(user_id, -product.price_coins, 'spent', 'store_purchase', {'product_id': product_id})

        # Cria compra
        purchase = StorePurchase.create(user_id, product_id, product.price_coins)

        # Entrega produto (lógica específica por tipo)
        deliver_product(user_id, product)

        wallet.save()
        return purchase
```

---

## 11. PERGUNTAS PARA O CLIENTE

1. **Coins iniciais**: Aluno ganha coins ao fazer signup? (ex: 50 coins de boas-vindas para comprar 1º item)
2. **Preços dinâmicos**: Produtos podem ter promoção (ex: 50% off em coins temporariamente)?
3. **Gifting**: Aluno pode enviar coins para outro aluno?
4. **Cash out**: Aluno pode trocar coins por dinheiro real? (ou é só moeda virtual?)
5. **Power-ups**: Quais power-ups são prioritários v1? (Freeze streak, XP boost, outros?)
6. **Ligas**: Competição semanal ou mensal? Resetar toda semana?

---

## 12. ROADMAP

### v1 (MVP) - 3 meses
- ✅ Sistema XP + Coins básico
- ✅ Streak diário
- ✅ Loja de recompensas (4 tipos: desconto, digital, personalização, power-up)
- ✅ Ligas (Bronze → Diamante)
- ✅ Daily goals

### v1.1 (Avançado) - 6 meses
- Time bonus
- Combo multiplier
- Produtos sazonais/limitados
- Gifting de coins
- Leaderboard global

### v2 (Social) - 12 meses
- Competições entre cohorts
- Desafios semanais
- Achievements complexos (ex: "Complete 100 lições sem errar")
- NFTs/Badges on-chain (opcional)

---

**Criado por**: Gamification Designer
**Revisado por**: [Aguardando]
**Aprovado por**: [Aguardando validação do cliente]
