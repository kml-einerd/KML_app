# Sistema de Gamificação: Economia Coins

**Versão**: 3.0 (REFORMULAÇÃO COMPLETA)
**Data**: 2025-12-27
**Status**: OFICIAL (baseado em resposta do cliente)
**Mudança Crítica**: **REMOVIDO XP** - Sistema unificado apenas com Coins

---

## MUDANÇA CRÍTICA ARQUITETURAL

### Cliente Disse

✅ **"Apenas Coins (não existe XP) - vamos simplificar, coins já são uma boa base"**

### Decisão Final

**ANTES** (arquitetura antiga):
- XP: Progresso (acumula, não gasta)
- Coins: Moeda (gasta na loja)

**AGORA** (arquitetura nova):
- **Apenas Coins** (sistema unificado)
- Coins acumulados (lifetime) = progresso
- Coins disponíveis (saldo) = moeda da loja

---

## 1. VISÃO GERAL

### 1.1. Sistema Unificado de Coins

**Coins servem para 2 propósitos**:

1. **Progresso** (Coins acumulados lifetime)
   - Nunca diminui (mesmo gastando)
   - Usado para níveis/badges
   - Mostra evolução do aluno

2. **Moeda** (Coins disponíveis/saldo)
   - Diminui ao comprar na loja
   - Recarrega ao ganhar mais coins
   - Usado para gastar na Loja de Recompensas

---

### 1.2. Exemplo Prático

**Aluno ao longo do tempo**:

| Ação | Coins Ganhos | Coins Lifetime | Coins Saldo | Nível |
|------|--------------|----------------|-------------|-------|
| Completar lesson 1 | +10 | 10 | 10 | Bronze |
| Completar lesson 2 | +10 | 20 | 20 | Bronze |
| Comprar tema escuro na loja (-50 coins) | 0 | 20 | -30 (saldo negativo não permitido) | Bronze |
| Completar lesson 3 | +10 | 30 | 10 (30 total - 50 gasto + 30 ganho) | Bronze |
| Completar track 1 (+50 coins) | +50 | 80 | 60 | Bronze |
| Comprar ebook (-100 coins) | 0 | 80 | -40 (não tem saldo suficiente) | Bronze |
| Completar mais 5 lessons (+50 coins) | +50 | 130 | 110 | **Prata** (nível sobe!) |

**Explicação**:
- **Coins Lifetime**: 130 (nunca diminui, base para nível)
- **Coins Saldo**: 110 (pode gastar na loja)
- **Nível**: Prata (baseado em 130 coins lifetime)

---

## 2. COMO GANHAR COINS

### 2.1. Tabela de Ganhos

**Cliente disse**: "precisa ter um sistema de gamificação com múltiplas formas inteligentes de bonificar, tipo o Duolingo"

| Ação | Coins Base | Coins com Bonus | Observações |
|------|------------|-----------------|-------------|
| **Completar Beat** | 1 coin | 1-2 coins | +1 coin se acerto perfeito |
| **Checkpoint correto (1ª tentativa)** | 2 coins | 2-5 coins | +3 coins se streak de 5+ acertos |
| **Completar Aula Interativa** | 10 coins | 10-20 coins | +10 coins se 100% acertos |
| **Completar Missão Diária** | 5 coins | 5-15 coins | +10 coins se completar antes das 12h |
| **Completar Atividade Interativa** | 15 coins | 15-30 coins | +15 coins se perfect score |
| **Completar Review SRS** | 5 coins | 5-10 coins | +5 coins se 100% recall |
| **Manter Streak (por dia)** | 3 coins | 3-10 coins | +7 coins se streak 7+ dias |
| **Streak de 7 dias** | 50 coins | 50 coins | Bonus semanal (milestone) |
| **Streak de 30 dias** | 200 coins | 200 coins | Bonus mensal (milestone) |
| **Completar Daily Goal** | 10 coins | 10 coins | Meta diária (ex: 1 lição/dia) |
| **Subir de Nível** | 20 coins | 20-100 coins | Escalona: Nível 10 = 100 coins |
| **Ganhar Badge** | 25 coins | 25 coins | Bonus por conquista |
| **Completar Estação** | 100 coins | 100-200 coins | +100 coins se 100% conclusão |
| **Convidar amigo (referral)** | 50 coins | 50 coins | Quando amigo completa 1ª lição |
| **Responder pesquisa NPS** | 10 coins | 10 coins | Feedback para criador |

---

### 2.2. Sistema de Streaks (Chave para Coins)

**Streak**: Dias consecutivos que aluno completa Daily Goal

**Daily Goal Configurável**:
- Casual: 1 lição/dia (10 coins/dia)
- Regular: 2 lições/dia (20 coins/dia)
- Sério: 3 lições/dia (30 coins/dia)
- Intenso: 5+ lições/dia (50+ coins/dia)

**Bonificação por Streak**:

| Streak | Bonus Coins Diário | Bonus Milestone | Total Acumulado |
|--------|-------------------|-----------------|-----------------|
| 1 dia | +3 coins | - | 3 coins |
| 3 dias | +5 coins/dia | +10 coins | 3 + 3 + 3 + 10 = 19 coins |
| 7 dias | +7 coins/dia | +50 coins | ~70 coins (7 dias) |
| 14 dias | +10 coins/dia | +100 coins | ~240 coins (14 dias) |
| 30 dias | +15 coins/dia | +200 coins | ~650 coins (30 dias) |
| 100 dias | +20 coins/dia | +500 coins | ~2.500 coins (100 dias) |

**Visual no Learner App**:
```
┌─────────────────────────────────────────────┐
│  🔥 Streak: 7 dias                          │
│  ███████░░░░░░░░░░░░░░░░░░░░░░              │
│  Próximo milestone: 14 dias (+100 coins)    │
└─────────────────────────────────────────────┘
```

---

### 2.3. Perfect Score Bonus

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
│  +30 coins (+15 bonus perfect!)             │
└─────────────────────────────────────────────┘
```

---

## 3. NÍVEIS (BASEADOS EM COINS LIFETIME)

### 3.1. Como Funcionam Níveis

**Sistema**: Níveis baseados em **Coins acumulados (lifetime)**

**Por que lifetime?**
- Aluno não "perde nível" ao gastar coins na loja
- Incentiva gastar coins sem medo de perder progresso
- Mostra evolução real (total de coins já ganhos)

---

### 3.2. Tabela de Níveis

| Nível | Coins Lifetime Necessários | Título | Ícone |
|-------|---------------------------|--------|-------|
| 1 | 0-100 coins | Bronze | 🥉 |
| 2 | 101-500 coins | Prata | 🥈 |
| 3 | 501-2.000 coins | Ouro | 🥇 |
| 4 | 2.001-10.000 coins | Diamante | 💎 |
| 5 | 10.001+ coins | Lenda | 👑 |

**Visual no Learner App**:
```
┌─────────────────────────────────────────────┐
│  💎 Nível Diamante                          │
│  ████████████████████░░░░░░░ 65%            │
│  5.200 / 10.000 coins                       │
│  Faltam 4.800 coins para Lenda 👑           │
└─────────────────────────────────────────────┘
```

---

### 3.3. Recompensas por Level Up

**Ao subir de nível, aluno ganha**:
- Bonus de coins (escalona com nível)
- Badge especial
- Acesso a produtos exclusivos na loja (ex: produtos "só para Diamante+")

| Nível | Bonus Coins | Badge | Produto Desbloqueado |
|-------|-------------|-------|----------------------|
| Bronze → Prata | +20 coins | "Aprendiz" | Tema Escuro |
| Prata → Ouro | +50 coins | "Dedicado" | Avatares Premium |
| Ouro → Diamante | +100 coins | "Mestre" | Certificado Premium |
| Diamante → Lenda | +200 coins | "Lenda" | Todos os produtos |

---

## 4. LOJA DE RECOMPENSAS (COMO GASTAR COINS)

**Cliente disse**: "é configurado no admin do infoapp e não no modo studio e seria pelo usuario administrador"

### 4.1. Tipos de Produtos

| Tipo | Exemplos | Preço Sugerido (Coins) |
|------|----------|------------------------|
| **Desconto** | 10% off em produto do criador | 50-100 |
| **Produto Físico** (v1.1) | Camiseta, caneca, livro impresso | 500-1000 |
| **Produto Digital** | Ebook bonus, template, certificado premium | 100-300 |
| **Personalização** | Tema escuro, avatar premium, efeitos sonoros | 50-200 |
| **Power-ups** | Freeze streak (1 dia), Double coins (1 dia) | 30-100 |
| **Conteúdo Exclusivo** | Módulo avançado, aula bônus | 200-500 |

---

### 4.2. Configuração pelo Criador (InfoApp Admin Panel)

**Tela: Configurar Loja de Recompensas**

Criador pode:
- CRUD de produtos (nome, descrição, imagem, preço em coins)
- Definir estoque (limitado/ilimitado)
- Ativar/desativar produtos
- Ver histórico de compras
- Definir regras (ex: produto só aparece para alunos nível 3+)

**Estética**: Amazon/Mercado Livre simplificada (card de produto com imagem, nome, preço, botão "Comprar")

---

### 4.3. UX no Learner App

**Tela: Loja de Recompensas**

```
┌─────────────────────────────────────────────┐
│  💰 Seus Coins: 245 (saldo disponível)      │
│  📊 Coins Lifetime: 1.850 (Nível Ouro 🥇)   │
│  ─────────────────────────────────────────  │
│  [Card Produto 1]  [Card Produto 2]         │
│  Tema Escuro       Certificado Premium      │
│  50 coins          100 coins                │
│  [Comprar]         [Comprar]                │
└─────────────────────────────────────────────┘
```

**Fluxo de compra**:
1. Aluno clica "Comprar"
2. Modal: "Confirmar compra? -50 coins" (mostra saldo atual e saldo após compra)
3. Confirma → Coins são debitados do **saldo**, produto é entregue
4. Notificação: "Tema Escuro ativado! 🎉"
5. **Coins lifetime NÃO diminui** (apenas saldo diminui)

---

## 5. ECONOMIA CIRCULAR: GANHAR vs GASTAR

### 5.1. Objetivo: Engajamento Contínuo

**Problema**: Se aluno acumula muitos coins e compra tudo, perde motivação

**Solução**: Economia balanceada com produtos novos sazonais

---

### 5.2. Tabela de Ganhos vs Gastos (Estimativa)

**Aluno médio (1 lição/dia, streak 7 dias)**:

**Ganhos semanais**:
- 7 lições: 70 coins
- 7 checkpoints corretos: 14 coins
- Streak 7 dias: 50 coins (milestone)
- Daily goal (7x): 70 coins
- **Total**: ~204 coins/semana

**Gastos típicos**:
- Tema Escuro: 50 coins (1x)
- Power-up Freeze: 30 coins (1-2x/mês)
- Desconto 10%: 100 coins (1x/mês)
- **Total**: ~180 coins/mês

**Balanço**: Aluno acumula ~800 coins/mês e gasta ~180 → sobra ~620 coins → pode comprar produto digital (300 coins)

---

### 5.3. Produtos Sazonais/Limitados

**Estratégia**: Criador adiciona produtos limitados para incentivar gasto

**Exemplos**:
- "Camiseta Edição Limitada" (800 coins, 50 unidades)
- "Acesso antecipado Módulo 5" (500 coins, disponível por 7 dias)
- "Avatar de Natal" (100 coins, disponível em dezembro)

---

## 6. LIGAS E RANKING (COMPETIÇÃO SOCIAL)

### 6.1. Ligas por Coins Ganhos na Semana

**Sistema**: Alunos competem semanalmente em ligas baseadas em **Coins ganhos na semana** (não lifetime)

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
│  1. @joao        245 coins  ⬆️ Diamante      │
│  2. @maria       220 coins  ⬆️ Diamante      │
│  3. @pedro       205 coins  ⬆️ Diamante      │
│  ...                                        │
│  15. Você         80 coins  ➡️ Ouro          │
│  ...                                        │
│  48. @ana         20 coins  ⬇️ Prata         │
└─────────────────────────────────────────────┘
```

**Nota**: Ranking semanal é baseado em **Coins ganhos esta semana**, não lifetime total.

---

## 7. BADGES (CONQUISTAS)

### 7.1. Tipos de Badges

**Badges por Milestone**:
- 🌱 Iniciante (primeiro login)
- 🔥 Streak 7 dias
- ⭐ Streak 30 dias
- 💯 Perfect Score (primeira vez)
- 🎯 100 lições completadas
- 💎 Nível Diamante
- 👑 Nível Lenda

**Badges por Track**:
- Cada track completado dá badge específico
- Ex: "Mestre em Big Ideas" (completar track de Big Ideas)

---

### 7.2. Recompensa por Badge

**Ao ganhar badge, aluno recebe**:
- +25 coins
- Visual de celebração (confetti)
- Notificação

---

## 8. DAILY GOALS E PERSONALIZAÇÃO

### 8.1. Daily Goal Configurável

**Aluno escolhe meta diária**:
- Casual: 10 coins/dia (1 lição rápida)
- Regular: 20 coins/dia (2 lições)
- Sério: 30 coins/dia (3 lições)
- Intenso: 50+ coins/dia (5+ lições)

**Recompensa**: Completar Daily Goal → +10 coins

---

### 8.2. Notificações Inteligentes

**Streak em risco**:
- "🔥 Seu streak de 15 dias vai expirar em 2h! Complete 1 lição rápida para manter."

**Daily goal incompleto**:
- "Faltam 10 coins para sua meta diária! 5 min de Review SRS e você consegue 🎯"

**Milestone próximo**:
- "Você está a 50 coins de subir para Nível Diamante! 🚀"

---

## 9. ANTI-CHEAT E LIMITES

### 9.1. Prevenção de Exploits

**Problema**: Aluno pode tentar "farmar" coins fazendo/refazendo lições infinitamente

**Soluções**:
1. **Limite de refazer**: Lição só dá coins na 1ª conclusão (refazer dá 0 coins)
2. **Cooldown**: Não pode refazer lição nas primeiras 24h
3. **Rate limit**: Máximo 10 lições/dia dão coins (além disso, 0 coins)
4. **Detecção de padrões**: Se aluno completa lição em < 10 seg, não dá coins

---

## 10. IMPLEMENTAÇÃO TÉCNICA

### 10.1. Tabelas DB (InfoApp schema)

```sql
-- Wallet do aluno
CREATE TABLE user_wallets (
  user_id UUID PRIMARY KEY,
  coins_lifetime INTEGER DEFAULT 0,  -- Nunca diminui (progresso)
  coins_balance INTEGER DEFAULT 0,   -- Saldo atual (pode diminuir)
  level INTEGER DEFAULT 1,
  streak_days INTEGER DEFAULT 0,
  last_activity_date DATE,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Transações de Coins
CREATE TABLE coin_transactions (
  id UUID PRIMARY KEY,
  user_id UUID,
  amount INTEGER,  -- positivo = ganho, negativo = gasto
  type VARCHAR(20),  -- 'earned', 'spent', 'adjusted'
  source VARCHAR(50),  -- 'lesson_complete', 'store_purchase', etc.
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Produtos da loja
CREATE TABLE store_products (
  id UUID PRIMARY KEY,
  infoapp_id UUID,
  name VARCHAR(255),
  description TEXT,
  image_url VARCHAR(500),
  price_coins INTEGER,
  type VARCHAR(50),  -- 'discount', 'physical', 'digital', 'customization', 'powerup'
  stock_quantity INTEGER,  -- NULL = ilimitado
  min_level INTEGER,  -- Nível mínimo para comprar
  active BOOLEAN DEFAULT true,
  metadata JSONB,
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

---

### 10.2. Service Layer (Pseudocódigo)

```python
class CoinService:
    def award_coins(user_id, amount, source, metadata=None):
        """Dar coins ao aluno (ganho)"""
        wallet = UserWallet.get(user_id)

        # Atualizar lifetime (nunca diminui)
        wallet.coins_lifetime += amount

        # Atualizar saldo
        wallet.coins_balance += amount

        # Verificar level up
        new_level = calculate_level(wallet.coins_lifetime)
        if new_level > wallet.level:
            wallet.level = new_level
            award_coins(user_id, level_up_coins(new_level), 'level_up')
            award_badge(user_id, f'level_{new_level}')

        # Log transação
        CoinTransaction.create(user_id, amount, 'earned', source, metadata)

        wallet.save()
        return wallet

    def spend_coins(user_id, product_id):
        """Gastar coins na loja"""
        product = StoreProduct.get(product_id)
        wallet = UserWallet.get(user_id)

        # Verificar saldo
        if wallet.coins_balance < product.price_coins:
            raise InsufficientCoins()

        # Verificar nível mínimo
        if wallet.level < product.min_level:
            raise LevelTooLow()

        # Debitar coins (apenas do SALDO, lifetime NÃO diminui)
        wallet.coins_balance -= product.price_coins

        # Log transação
        CoinTransaction.create(
            user_id,
            -product.price_coins,
            'spent',
            'store_purchase',
            {'product_id': product_id}
        )

        # Criar compra
        purchase = StorePurchase.create(user_id, product_id, product.price_coins)

        # Entregar produto
        deliver_product(user_id, product)

        wallet.save()
        return purchase

    def calculate_level(coins_lifetime):
        """Calcular nível baseado em coins lifetime"""
        if coins_lifetime < 101:
            return 1  # Bronze
        elif coins_lifetime < 501:
            return 2  # Prata
        elif coins_lifetime < 2001:
            return 3  # Ouro
        elif coins_lifetime < 10001:
            return 4  # Diamante
        else:
            return 5  # Lenda
```

---

## 11. ROADMAP

### v1 (MVP) - 3 meses
- ✅ Sistema Coins unificado (lifetime + saldo)
- ✅ Streak diário
- ✅ Loja de recompensas (4 tipos: desconto, digital, personalização, power-up)
- ✅ Ligas (Bronze → Diamante)
- ✅ Daily goals
- ✅ Níveis baseados em Coins lifetime

### v1.1 (Avançado) - 6 meses
- Produtos sazonais/limitados
- Leaderboard global
- Badges avançados
- Double coins power-up

### v2 (Social) - 12 meses
- Competições entre cohorts
- Desafios semanais
- Achievements complexos

---

**Criado por**: Gamification Designer + Product Architect
**Baseado em**: Resposta do cliente (2025-12-27)
**Aprovado por**: Cliente ✅
