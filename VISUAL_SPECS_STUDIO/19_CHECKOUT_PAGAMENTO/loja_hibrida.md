# Loja Híbrida: Coins OU Reais

**Versão**: 1.0 (NOVO)
**Data**: 2025-12-27
**Status**: OFICIAL (baseado em resposta final do cliente)
**Mudança Crítica**: Loja aceita **Coins (gamificação) OU Reais (pagamento real)** - aluno escolhe

---

## RESPOSTA FINAL DO CLIENTE

✅ **"Loja aceita Coins + Reais (criador cobra por produtos)"**

**DECISÃO ARQUITETURAL**:
- Produto pode ter **2 preços opcionais**:
  - Preço em Coins (gamificação)
  - Preço em Reais (monetização real)
- Aluno **escolhe** qual forma de pagamento usar
- Se pagar com Coins → Apenas gamificação (criador não recebe $)
- Se pagar com Reais → Criador recebe dinheiro via Stripe

---

## 1. VISÃO GERAL

### 1.1. Sistema Híbrido de Pagamento

**Produto configurável com 3 opções**:
1. **Apenas Coins**: Produto virtual (personalização, power-up)
2. **Apenas Reais**: Produto físico ou digital com alto valor
3. **Coins OU Reais**: Aluno escolhe (produto tem 2 preços)

---

### 1.2. Exemplo Prático

**Produto: Certificado Premium**
```yaml
produto:
  nome: "Certificado Premium"
  preco_coins: 500        # Opcional (pode ser NULL)
  preco_reais: 29.90      # Opcional (pode ser NULL, BRL)
  tipo: "digital"
  aceita_coins: true
  aceita_reais: true
```

**Aluno vê na Loja**:
```
┌────────────────────────────────────────┐
│ 📜 Certificado Premium                 │
│                                        │
│ Certificado digital assinado pelo      │
│ criador com validade nacional          │
│                                        │
│ 💰 500 Coins  OU  💵 R$ 29,90          │
│                                        │
│ [Comprar com Coins] [Comprar por R$]  │
└────────────────────────────────────────┘
```

**Fluxos**:
- Clica "Comprar com Coins" → Validação de saldo → Debita coins → Entrega produto
- Clica "Comprar por R$" → Checkout Stripe → Pagamento confirmado → Entrega produto

---

## 2. TIPOS DE PRODUTOS E PRECIFICAÇÃO

### 2.1. Produtos Apenas Coins (Virtual)

**Características**:
- 100% gamificação (criador NÃO recebe dinheiro)
- Entrega automática
- Sem taxa da plataforma

**Exemplos**:
| Produto | Preço Coins | Tipo |
|---------|-------------|------|
| Tema Escuro | 50 | Personalização |
| Avatar Premium | 100 | Personalização |
| Freeze Streak (1 dia) | 30 | Power-up |
| Double Coins (24h) | 100 | Power-up |
| Efeitos Sonoros | 75 | Personalização |

**Config no Admin**:
```yaml
preco_coins: 50
preco_reais: null
aceita_coins: true
aceita_reais: false
```

---

### 2.2. Produtos Apenas Reais (Alto Valor)

**Características**:
- Monetização real (criador recebe pagamento)
- Entrega manual ou automática (depende do tipo)
- Plataforma cobra taxa (ex: 5-10%)

**Exemplos**:
| Produto | Preço Reais (BRL) | Tipo |
|---------|-------------------|------|
| Camiseta Exclusiva | R$ 79,90 | Físico |
| Livro Impresso | R$ 59,00 | Físico |
| Mentoria 1h | R$ 200,00 | Serviço |
| Curso Avançado | R$ 149,00 | Digital |

**Config no Admin**:
```yaml
preco_coins: null
preco_reais: 79.90
aceita_coins: false
aceita_reais: true
```

---

### 2.3. Produtos Híbridos (Coins OU Reais)

**Características**:
- Aluno escolhe como pagar
- Criador define **equivalência** entre Coins e Reais
- Produto tem 2 preços (ambos visíveis)

**Exemplos**:
| Produto | Preço Coins | Preço Reais | Tipo |
|---------|-------------|-------------|------|
| Certificado Premium | 500 | R$ 29,90 | Digital |
| Ebook Exclusivo | 300 | R$ 19,90 | Digital |
| Desconto 20% | 200 | R$ 15,00 | Desconto |
| Acesso Módulo Bônus | 1000 | R$ 49,00 | Conteúdo |

**Config no Admin**:
```yaml
preco_coins: 500
preco_reais: 29.90
aceita_coins: true
aceita_reais: true
```

**Estratégia de precificação**:
- Criador define preço em Reais PRIMEIRO (valor real do produto)
- Sistema sugere preço em Coins baseado em equivalência (ex: 1 Real = 15-20 Coins)
- Criador pode ajustar manualmente

**Exemplo de cálculo**:
- Certificado vale R$ 29,90
- Equivalência: 1 BRL = 16,7 Coins
- Sugestão: 500 Coins (29,90 × 16,7 = 499,83 ≈ 500)

---

## 3. UX/UI DA LOJA HÍBRIDA

### 3.1. Card de Produto (Learner App)

**Produto apenas Coins**:
```
┌────────────────────────────────────────┐
│ 🎨 Tema Escuro Premium                 │
│                                        │
│ Ative o modo escuro para estudar       │
│ de noite sem cansar a vista            │
│                                        │
│ 💰 50 Coins                            │
│                                        │
│ [Comprar com Coins]                    │
└────────────────────────────────────────┘
```

**Produto apenas Reais**:
```
┌────────────────────────────────────────┐
│ 👕 Camiseta Exclusiva                  │
│                                        │
│ Camiseta 100% algodão com logo do      │
│ InfoApp. Edição limitada!              │
│                                        │
│ 💵 R$ 79,90                            │
│                                        │
│ [Comprar por R$ 79,90]                 │
└────────────────────────────────────────┘
```

**Produto híbrido (Coins OU Reais)**:
```
┌────────────────────────────────────────┐
│ 📜 Certificado Premium                 │
│                                        │
│ Certificado digital assinado com       │
│ validade nacional                      │
│                                        │
│ 💰 500 Coins  OU  💵 R$ 29,90          │
│                                        │
│ [Comprar com Coins] [Comprar por R$]  │
└────────────────────────────────────────┘
```

---

### 3.2. Modal de Confirmação (Coins)

**Aluno clica "Comprar com Coins"**:
```
┌────────────────────────────────────────┐
│ ✕  Confirmar Compra                    │
├────────────────────────────────────────┤
│                                        │
│ 📜 Certificado Premium                 │
│                                        │
│ Preço: -500 coins                      │
│                                        │
│ Seu saldo atual: 1.245 coins           │
│ Saldo após compra: 745 coins           │
│                                        │
│ ⚠️  Coins gastos não podem ser         │
│    reembolsados                        │
│                                        │
│ ──────────────────────────────────────│
│              [Cancelar]  [Confirmar]   │
└────────────────────────────────────────┘
```

**Ao confirmar**:
1. Backend valida saldo (aluno tem 500+ coins?)
2. Debita 500 coins do **saldo** (coins lifetime NÃO diminui)
3. Registra compra na tabela `store_purchases`
4. Entrega produto (digital = link download, personalização = aplica automaticamente)
5. Toast: "Certificado comprado! 🎉 Confira seu email."

**Se saldo insuficiente**:
```
┌────────────────────────────────────────┐
│ ⚠️  Coins Insuficientes                │
├────────────────────────────────────────┤
│ Você precisa de 500 coins mas tem      │
│ apenas 245 coins.                      │
│                                        │
│ Faltam 255 coins. Continue estudando   │
│ para ganhar mais!                      │
│                                        │
│ 💡 Dica: Complete 3 lições para ganhar │
│    ~90 coins                           │
│                                        │
│ ──────────────────────────────────────│
│              [OK]  [Ver Como Ganhar]   │
└────────────────────────────────────────┘
```

---

### 3.3. Checkout (Reais)

**Aluno clica "Comprar por R$ 29,90"**:
1. Redireciona para **Stripe Checkout** (hosted)
2. Stripe exibe formulário de pagamento:
   - Cartão de crédito/débito
   - PIX (Brasil)
   - Boleto (Brasil)
3. Aluno preenche dados e paga
4. Stripe processa pagamento
5. Webhook notifica backend
6. Backend entrega produto
7. Aluno recebe email com produto

**Fluxo técnico** (ver `stripe_integration.md`)

---

## 4. CONFIGURAÇÃO NO INFOAPP ADMIN PANEL

### 4.1. Tela: Criar/Editar Produto

**Tab: Básico**
```
┌────────────────────────────────────────┐
│ BÁSICO                                 │
│ Nome: [Certificado Premium]            │
│ Descrição: [Campo de texto...]         │
│ Tipo: [Digital ▼]                      │
│ Imagem: [📷 Upload]                    │
│                                        │
│ PRECIFICAÇÃO                           │
│ ☑️  Aceita Coins                       │
│     Preço: [500] coins                 │
│                                        │
│ ☑️  Aceita Reais (BRL)                 │
│     Preço: [29.90] reais               │
│                                        │
│ 💡 Sugestão: 500 coins ≈ R$ 29,90      │
│    (equivalência: 1 BRL = 16,7 coins)  │
│                                        │
│ ⚠️  Produtos com preço em Reais        │
│    requerem conexão com Stripe         │
│    [Conectar Stripe]                   │
└────────────────────────────────────────┘
```

**Validações**:
- Produto PRECISA ter pelo menos 1 preço (Coins OU Reais)
- Se aceita Reais → criador PRECISA ter conectado Stripe
- Preço em Coins: mínimo 1, máximo 10.000
- Preço em Reais: mínimo R$ 1,00, máximo R$ 10.000,00

---

### 4.2. Conectar Stripe (Setup Inicial)

**Primeira vez que criador marca "Aceita Reais"**:
```
┌────────────────────────────────────────┐
│ ⚠️  Conectar Stripe                    │
├────────────────────────────────────────┤
│ Para vender produtos por Reais, você   │
│ precisa conectar sua conta Stripe.     │
│                                        │
│ Stripe é a plataforma de pagamentos    │
│ mais segura do mundo, usada por        │
│ Shopify, Uber, Amazon.                 │
│                                        │
│ ✅ Suporta cartão, PIX, boleto         │
│ ✅ Receba pagamentos em 2-7 dias       │
│ ✅ Taxa: 3,99% + R$ 0,39 por transação │
│                                        │
│ A plataforma cobra 5% de comissão      │
│ sobre vendas (R$ 29,90 → você recebe   │
│ ~R$ 27,41 após taxas).                 │
│                                        │
│ ──────────────────────────────────────│
│     [Cancelar]  [Conectar com Stripe] │
└────────────────────────────────────────┘
```

**Ao clicar "Conectar com Stripe"**:
1. Redireciona para Stripe Connect OAuth
2. Criador cria conta Stripe (ou conecta existente)
3. Stripe verifica identidade (KYC)
4. Stripe retorna para plataforma com `stripe_account_id`
5. Backend salva `stripe_account_id` do criador
6. Toast: "Stripe conectado! Agora você pode vender produtos por Reais."

---

## 5. REGRAS DE NEGÓCIO

### 5.1. Equivalência Coins vs Reais

**Problema**: Como definir equivalência entre Coins (gamificação) e Reais (moeda real)?

**Solução**: Criador define preços independentemente
- Preço em Coins: Baseado em quanto aluno ganha estudando (ex: 1 lição = 10 coins → certificado = 50 lições)
- Preço em Reais: Baseado no valor real do produto (ex: R$ 29,90)

**Não há conversão automática** (1 Coin ≠ R$ X fixo)
- Sistema SUGERE equivalência (apenas orientação)
- Criador pode ajustar livremente

**Exemplo**:
- Certificado: 500 Coins OU R$ 29,90
- Equivalência implícita: 1 BRL ≈ 16,7 Coins
- MAS é apenas sugestão (criador pode fazer 500 Coins OU R$ 50,00 se quiser)

---

### 5.2. Taxa da Plataforma

**Produtos pagos com Coins**: 0% (100% gamificação)

**Produtos pagos com Reais**:
- Stripe cobra: 3,99% + R$ 0,39 por transação
- Plataforma cobra: 5% de comissão (configurável)

**Exemplo** (Certificado R$ 29,90):
1. Aluno paga: R$ 29,90
2. Stripe desconta taxa: R$ 1,59 (5,3%)
3. Plataforma desconta comissão: R$ 1,50 (5%)
4. **Criador recebe**: R$ 26,81 (~89,7%)

**Split payment** (ver `split_payment.md`)

---

### 5.3. Limite de Compras

**Por aluno**:
- Pode comprar mesmo produto com Coins E com Reais? **NÃO**
- Sistema registra compra (independente do método de pagamento)
- Se produto tem limite "1 por aluno" → só pode comprar 1 vez (Coins OU Reais)

**Exemplo**:
- Produto: Certificado (limite: 1 por aluno)
- Aluno compra com Coins → produto some da loja (já comprado)
- Aluno NÃO pode comprar novamente com Reais (limite atingido)

---

### 5.4. Reembolso

**Coins**: Não há reembolso automático
- v1: Coins gastos não voltam
- v1.1: Criador pode reembolsar manualmente (creditar coins de volta via Admin Panel)

**Reais**: Segue política Stripe
- Aluno pode solicitar reembolso via Stripe (até 60 dias)
- Criador avalia e aprova/rejeita via Admin Panel
- Se aprovado → Stripe reembolsa dinheiro, plataforma estorna comissão

---

## 6. FLUXO COMPLETO (EXEMPLO)

### 6.1. Criador Configurando Loja

1. Criador acessa InfoApp Admin → "Loja de Recompensas"
2. Clica "+ Novo Produto"
3. Preenche:
   - Nome: "Certificado Premium"
   - Descrição: "Certificado digital assinado..."
   - Tipo: Digital
   - Imagem: Upload
4. Precificação:
   - ☑️ Aceita Coins: 500
   - ☑️ Aceita Reais: R$ 29,90
5. Sistema avisa: "Conectar Stripe para vender por Reais"
6. Criador clica "Conectar Stripe" → OAuth → Conta conectada
7. Salva produto → Produto ativo na loja

---

### 6.2. Aluno Comprando com Coins

1. Aluno acessa Learner App → "Loja de Recompensas"
2. Vê produto: "500 Coins OU R$ 29,90"
3. Clica "Comprar com Coins"
4. Modal: "Confirmar compra? -500 coins"
5. Confirma
6. Backend:
   - Valida saldo (tem 1.245 coins → OK)
   - Debita 500 coins do saldo
   - Registra compra (método: `coins`)
   - Envia email com certificado (link download)
7. Toast: "Certificado comprado! 🎉"
8. Saldo atualizado: 745 coins

---

### 6.3. Aluno Comprando com Reais

1. Aluno acessa Learner App → "Loja de Recompensas"
2. Vê produto: "500 Coins OU R$ 29,90"
3. Clica "Comprar por R$ 29,90"
4. Redireciona para Stripe Checkout
5. Stripe exibe formulário (cartão/PIX)
6. Aluno escolhe PIX → Gera QR Code
7. Aluno paga PIX
8. Stripe confirma pagamento → Webhook
9. Backend:
   - Registra compra (método: `brl`, payment_id: `pi_xxx`)
   - Envia email com certificado
   - Notifica criador (venda de R$ 29,90)
10. Aluno recebe email: "Certificado adquirido!"
11. Criador recebe pagamento em 2-7 dias (R$ 26,81 após taxas)

---

## 7. IMPLEMENTAÇÃO TÉCNICA

### 7.1. Tabela DB (Produtos)

```sql
CREATE TABLE store_products (
  id UUID PRIMARY KEY,
  infoapp_id UUID,
  name VARCHAR(255),
  description TEXT,
  image_url VARCHAR(500),
  type VARCHAR(50),  -- 'digital', 'physical', 'customization', 'powerup', 'discount'

  -- PREÇOS HÍBRIDOS (ambos opcionais, mas pelo menos 1 obrigatório)
  price_coins INTEGER,        -- NULL se não aceita Coins
  price_brl DECIMAL(10,2),    -- NULL se não aceita Reais
  accepts_coins BOOLEAN DEFAULT false,
  accepts_brl BOOLEAN DEFAULT false,

  stock_quantity INTEGER,
  min_level INTEGER,
  active BOOLEAN DEFAULT true,
  delivery_config JSONB,
  restrictions JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Constraint**: `CHECK (price_coins IS NOT NULL OR price_brl IS NOT NULL)`

---

### 7.2. Tabela DB (Compras)

```sql
CREATE TABLE store_purchases (
  id UUID PRIMARY KEY,
  user_id UUID,
  product_id UUID,
  payment_method VARCHAR(20),  -- 'coins' ou 'brl'

  -- Se método = coins
  price_paid_coins INTEGER,

  -- Se método = brl
  price_paid_brl DECIMAL(10,2),
  stripe_payment_id VARCHAR(255),  -- payment_intent_id
  stripe_status VARCHAR(50),       -- 'pending', 'succeeded', 'refunded'

  status VARCHAR(20),  -- 'completed', 'pending', 'cancelled', 'refunded'
  delivered_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

### 7.3. Service Layer (Pseudocódigo)

```python
class StoreService:
    def purchase_with_coins(user_id, product_id):
        """Comprar produto com Coins"""
        product = StoreProduct.get(product_id)

        # Validações
        if not product.accepts_coins:
            raise PaymentMethodNotAllowed("Produto não aceita Coins")

        wallet = UserWallet.get(user_id)
        if wallet.coins_balance < product.price_coins:
            raise InsufficientCoins()

        # Debitar coins
        CoinService.spend_coins(user_id, product_id)

        # Registrar compra
        purchase = StorePurchase.create(
            user_id=user_id,
            product_id=product_id,
            payment_method='coins',
            price_paid_coins=product.price_coins,
            status='completed'
        )

        # Entregar produto
        deliver_product(user_id, product)

        return purchase

    def purchase_with_brl(user_id, product_id):
        """Comprar produto com Reais (inicia checkout Stripe)"""
        product = StoreProduct.get(product_id)

        # Validações
        if not product.accepts_brl:
            raise PaymentMethodNotAllowed("Produto não aceita Reais")

        infoapp = InfoApp.get(product.infoapp_id)
        if not infoapp.stripe_account_id:
            raise StripeNotConnected("Criador não conectou Stripe")

        # Criar checkout Stripe
        checkout_url = StripeService.create_checkout_session(
            product=product,
            user_id=user_id,
            success_url=f'/store/success/{product_id}',
            cancel_url=f'/store/cancel'
        )

        # Registrar compra (status pending)
        purchase = StorePurchase.create(
            user_id=user_id,
            product_id=product_id,
            payment_method='brl',
            price_paid_brl=product.price_brl,
            status='pending'
        )

        return checkout_url  # Frontend redireciona

    def handle_stripe_webhook(event):
        """Webhook Stripe (pagamento confirmado)"""
        if event.type == 'checkout.session.completed':
            session = event.data.object

            # Encontrar compra
            purchase = StorePurchase.get_by_stripe_session(session.id)

            # Atualizar status
            purchase.status = 'completed'
            purchase.stripe_payment_id = session.payment_intent
            purchase.stripe_status = 'succeeded'
            purchase.save()

            # Entregar produto
            deliver_product(purchase.user_id, purchase.product)

            # Notificar criador
            notify_creator_sale(purchase)
```

---

## 8. ANALYTICS E MÉTRICAS

**Eventos a trackear**:
- `store_product_viewed`: Aluno visualiza produto (param: `has_coins_price`, `has_brl_price`)
- `store_purchase_initiated`: Aluno clica em botão comprar (param: `payment_method`)
- `store_purchase_completed`: Compra finalizada (param: `payment_method`, `price_paid`)
- `store_insufficient_coins`: Aluno tenta comprar sem saldo
- `store_checkout_abandoned`: Aluno abandona checkout Stripe

**KPIs importantes**:
- % produtos com Coins vs Reais vs Híbrido
- Taxa de conversão Coins vs Reais
- Ticket médio (Coins e Reais separadamente)
- % alunos que preferem Coins vs Reais (quando produto é híbrido)

---

## 9. PRÓXIMOS PASSOS

1. ✅ Definir arquitetura loja híbrida (COMPLETO)
2. [ ] Implementar integração Stripe (ver `stripe_integration.md`)
3. [ ] Criar UI no InfoApp Admin (configurar preços Coins/Reais)
4. [ ] Criar UI no Learner App (2 botões de compra)
5. [ ] Implementar webhook Stripe
6. [ ] Testar fluxo completo (Coins e Reais)

---

**Criado por**: Product Lead + Tech Lead
**Baseado em**: Resposta final do cliente (2025-12-27)
**Aprovado por**: Cliente ✅
**Relacionado**: `stripe_integration.md`, `split_payment.md`
