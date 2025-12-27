# Integração Stripe (Checkout e Pagamentos)

**Versão**: 1.0 (NOVO)
**Data**: 2025-12-27
**Status**: OFICIAL (baseado em resposta final do cliente)
**Mudança Crítica**: Checkout com Stripe para produtos pagos em Reais

---

## CONTEXTO

**Cliente disse**: ✅ "Loja aceita Coins + Reais (criador cobra por produtos)"

**DECISÃO TÉCNICA**:
- Integração com **Stripe** (padrão internacional + suporte BR)
- Criador conecta Stripe Connect no InfoApp Admin
- Split payment: Plataforma cobra taxa (5% configurável)
- Suporte: Cartão, PIX, Boleto (Brasil)

---

## 1. VISÃO GERAL DA INTEGRAÇÃO

### 1.1. Por que Stripe?

**Vantagens**:
- ✅ Suporta Brasil (cartão, PIX, boleto)
- ✅ Stripe Connect (split payment nativo)
- ✅ Webhooks confiáveis (99,9% uptime)
- ✅ Documentação excelente
- ✅ SDKs oficiais (Node.js, Python, etc.)
- ✅ Compliance PCI DSS (segurança de pagamentos)
- ✅ Sem necessidade de certificação adicional

**Taxas Stripe Brasil**:
- Cartão de crédito: 3,99% + R$ 0,39 por transação
- PIX: 0,99% (sem taxa fixa)
- Boleto: R$ 2,99 por transação

---

### 1.2. Arquitetura (Stripe Connect)

**Stripe Connect**: Plataforma conecta múltiplos criadores

```
┌─────────────────────────────────────────────────────┐
│                   PLATAFORMA                        │
│                (Stripe Account Master)              │
│                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐│
│  │  Criador A   │  │  Criador B   │  │ Criador C ││
│  │ (Connected)  │  │ (Connected)  │  │(Connected)││
│  └──────────────┘  └──────────────┘  └───────────┘│
└─────────────────────────────────────────────────────┘
         ▲                  ▲                  ▲
         │                  │                  │
    [Aluno 1]          [Aluno 2]          [Aluno 3]
```

**Fluxo**:
1. Aluno compra produto do Criador A por R$ 29,90
2. Pagamento vai para **Stripe Master Account** (plataforma)
3. Stripe faz **split automático**:
   - 5% (R$ 1,50) → Plataforma
   - 95% (R$ 28,40) → Criador A (menos taxa Stripe)
4. Criador A recebe R$ 26,81 na conta Stripe dele

---

## 2. SETUP STRIPE CONNECT (CRIADOR)

### 2.1. Fluxo OAuth (Primeira Vez)

**Passo 1: Criador clica "Conectar Stripe"** (InfoApp Admin)
```
┌────────────────────────────────────────┐
│ ⚠️  Conectar Stripe                    │
├────────────────────────────────────────┤
│ Para vender produtos por Reais, você   │
│ precisa conectar sua conta Stripe.     │
│                                        │
│ ✅ Receba pagamentos em 2-7 dias       │
│ ✅ Suporta cartão, PIX, boleto         │
│ ✅ Taxa: 3,99% + R$ 0,39 (Stripe)      │
│ ✅ Comissão plataforma: 5%             │
│                                        │
│ ──────────────────────────────────────│
│     [Cancelar]  [Conectar com Stripe] │
└────────────────────────────────────────┘
```

**Passo 2: Redireciona para Stripe OAuth**
```
URL: https://connect.stripe.com/oauth/authorize?
  client_id=ca_xxx
  &state={creator_id}
  &scope=read_write
  &response_type=code
  &redirect_uri=https://plataforma.com/stripe/callback
```

**Passo 3: Criador autoriza no Stripe**
- Se não tem conta Stripe → cria nova conta
- Se já tem conta → conecta existente
- Stripe verifica identidade (KYC): CPF/CNPJ, dados bancários

**Passo 4: Stripe retorna código de autorização**
```
https://plataforma.com/stripe/callback?
  code=ac_xxxxx
  &state={creator_id}
```

**Passo 5: Backend troca código por `stripe_account_id`**
```python
# Backend (Node.js exemplo)
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

app.get('/stripe/callback', async (req, res) => {
  const { code, state } = req.query;
  const creator_id = state;

  // Trocar código por account_id
  const response = await stripe.oauth.token({
    grant_type: 'authorization_code',
    code: code,
  });

  const stripe_account_id = response.stripe_user_id;

  // Salvar no DB
  await db.creators.update(creator_id, {
    stripe_account_id: stripe_account_id,
    stripe_connected_at: new Date(),
  });

  res.redirect('/admin/store?stripe_connected=true');
});
```

**Passo 6: Criador retorna para Admin Panel**
- Toast: "Stripe conectado! Agora você pode vender produtos por Reais."
- Badge: "Stripe ✅ Conectado"

---

### 2.2. Dados Salvos no DB (Criador)

```sql
CREATE TABLE infoapp_creators (
  id UUID PRIMARY KEY,
  infoapp_id UUID,
  stripe_account_id VARCHAR(255),  -- acct_xxxxx
  stripe_connected_at TIMESTAMP,
  stripe_charges_enabled BOOLEAN,  -- Pode receber pagamentos?
  stripe_payouts_enabled BOOLEAN,  -- Pode receber saques?
  stripe_details_submitted BOOLEAN, -- KYC completo?
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Validação antes de vender**:
- `stripe_account_id IS NOT NULL`
- `stripe_charges_enabled = true`
- `stripe_details_submitted = true`

Se KYC não completo → Stripe bloqueia pagamentos até criador completar

---

## 3. CHECKOUT STRIPE (ALUNO COMPRANDO)

### 3.1. Fluxo Completo

**Passo 1: Aluno clica "Comprar por R$ 29,90"** (Learner App)

**Passo 2: Backend cria Checkout Session**
```python
# Backend (Python/FastAPI exemplo)
from stripe import checkout

@app.post("/api/store/purchase/{product_id}")
async def purchase_product(product_id: str, user_id: str):
    product = db.products.get(product_id)
    creator = db.creators.get_by_infoapp(product.infoapp_id)

    # Validações
    if not product.accepts_brl:
        raise HTTPException(400, "Produto não aceita Reais")
    if not creator.stripe_account_id:
        raise HTTPException(400, "Criador não conectou Stripe")

    # Criar Checkout Session
    session = checkout.Session.create(
        payment_method_types=['card', 'boleto'],  # Cartão + Boleto
        line_items=[{
            'price_data': {
                'currency': 'brl',
                'product_data': {
                    'name': product.name,
                    'description': product.description,
                    'images': [product.image_url],
                },
                'unit_amount': int(product.price_brl * 100),  # Centavos
            },
            'quantity': 1,
        }],
        mode='payment',
        success_url=f'https://plataforma.com/store/success?session_id={{CHECKOUT_SESSION_ID}}',
        cancel_url='https://plataforma.com/store/cancel',
        metadata={
            'product_id': product_id,
            'user_id': user_id,
            'infoapp_id': product.infoapp_id,
        },
        # SPLIT PAYMENT: 5% vai para plataforma, resto para criador
        payment_intent_data={
            'application_fee_amount': int(product.price_brl * 100 * 0.05),  # 5%
            'transfer_data': {
                'destination': creator.stripe_account_id,
            },
        },
    )

    # Salvar compra (status pending)
    purchase = db.purchases.create(
        user_id=user_id,
        product_id=product_id,
        payment_method='brl',
        price_paid_brl=product.price_brl,
        stripe_session_id=session.id,
        status='pending',
    )

    return {'checkout_url': session.url}
```

**Passo 3: Frontend redireciona para Stripe**
```javascript
// Frontend (React exemplo)
const response = await fetch(`/api/store/purchase/${productId}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ user_id: currentUserId }),
});

const { checkout_url } = await response.json();

// Redirecionar para Stripe Checkout
window.location.href = checkout_url;
```

**Passo 4: Stripe exibe formulário de pagamento**
```
┌────────────────────────────────────────┐
│ 🔐 Stripe Checkout                     │
├────────────────────────────────────────┤
│ Certificado Premium                    │
│ R$ 29,90                               │
│                                        │
│ Email: aluno@example.com               │
│                                        │
│ Método de pagamento:                   │
│ (●) Cartão de crédito                  │
│ ( ) PIX                                │
│ ( ) Boleto                             │
│                                        │
│ Número do cartão:                      │
│ [____-____-____-____]                  │
│                                        │
│ Validade:  [MM/AA]   CVV: [___]        │
│                                        │
│ [Pagar R$ 29,90]                       │
└────────────────────────────────────────┘
```

**Passo 5: Aluno paga**
- Se PIX → Stripe gera QR Code
- Se Boleto → Stripe gera PDF
- Se Cartão → Stripe processa imediatamente

**Passo 6: Stripe processa pagamento**
- Valida cartão/PIX/boleto
- Faz split automático (95% criador, 5% plataforma)
- Envia webhook para backend

**Passo 7: Webhook confirma pagamento** (ver seção 4)

**Passo 8: Aluno retorna para plataforma**
```
URL: https://plataforma.com/store/success?session_id=cs_xxxxx
```

**Passo 9: Frontend mostra confirmação**
```
┌────────────────────────────────────────┐
│ ✅ Compra realizada com sucesso!       │
├────────────────────────────────────────┤
│ Certificado Premium                    │
│                                        │
│ Você comprou:                          │
│ 📜 Certificado Premium                 │
│ Pagamento: R$ 29,90 (PIX)              │
│                                        │
│ 📧 Enviamos o certificado para seu     │
│    email: aluno@example.com            │
│                                        │
│ [Voltar para Loja]  [Ver Certificado] │
└────────────────────────────────────────┘
```

---

### 3.2. Suporte a PIX

**PIX** é método de pagamento instantâneo brasileiro (obrigatório)

**Ativar PIX no Stripe**:
```python
session = checkout.Session.create(
    payment_method_types=['card', 'boleto'],
    # PIX configurado via Stripe Dashboard
    # Stripe gera QR Code automaticamente
    ...
)
```

**Fluxo PIX**:
1. Aluno escolhe PIX no checkout
2. Stripe gera QR Code
3. Aluno escaneia com app do banco
4. Paga via PIX (instantâneo)
5. Stripe confirma pagamento em ~10 segundos
6. Webhook notifica backend
7. Produto é entregue

**Taxa PIX**: 0,99% (sem taxa fixa) → mais barato que cartão

---

## 4. WEBHOOKS STRIPE (PAGAMENTO CONFIRMADO)

### 4.1. Eventos Importantes

**Eventos a ouvir**:
- `checkout.session.completed`: Checkout finalizado (pagamento confirmado)
- `payment_intent.succeeded`: Pagamento processado com sucesso
- `payment_intent.payment_failed`: Pagamento falhou
- `charge.refunded`: Reembolso emitido

---

### 4.2. Implementação Webhook

**Passo 1: Configurar endpoint no Stripe Dashboard**
```
URL do webhook: https://plataforma.com/webhooks/stripe
Eventos: checkout.session.completed, payment_intent.succeeded, charge.refunded
```

**Passo 2: Implementar endpoint**
```python
# Backend (Python/FastAPI exemplo)
import stripe
from fastapi import Request, HTTPException

@app.post("/webhooks/stripe")
async def stripe_webhook(request: Request):
    payload = await request.body()
    sig_header = request.headers.get('stripe-signature')

    # Verificar assinatura (segurança)
    try:
        event = stripe.Webhook.construct_event(
            payload, sig_header, STRIPE_WEBHOOK_SECRET
        )
    except ValueError:
        raise HTTPException(400, "Invalid payload")
    except stripe.error.SignatureVerificationError:
        raise HTTPException(400, "Invalid signature")

    # Processar evento
    if event.type == 'checkout.session.completed':
        session = event.data.object

        # Encontrar compra
        purchase = db.purchases.get_by_stripe_session(session.id)

        # Atualizar status
        purchase.status = 'completed'
        purchase.stripe_payment_id = session.payment_intent
        purchase.stripe_status = 'succeeded'
        purchase.save()

        # Entregar produto
        deliver_product(purchase.user_id, purchase.product_id)

        # Notificar criador
        notify_creator_sale(purchase)

        # Enviar email para aluno
        send_email(
            to=purchase.user_email,
            subject='Compra confirmada!',
            template='purchase_success',
            data={'product': product, 'purchase': purchase}
        )

    elif event.type == 'charge.refunded':
        refund = event.data.object

        # Processar reembolso
        purchase = db.purchases.get_by_payment_id(refund.payment_intent)
        purchase.status = 'refunded'
        purchase.save()

        # Reverter entrega (se possível)
        revoke_product(purchase.user_id, purchase.product_id)

    return {'status': 'success'}
```

**Passo 3: Testar webhook localmente (Stripe CLI)**
```bash
# Instalar Stripe CLI
brew install stripe/stripe-cli/stripe

# Escutar webhooks localmente
stripe listen --forward-to localhost:8000/webhooks/stripe

# Trigger evento de teste
stripe trigger checkout.session.completed
```

---

### 4.3. Segurança Webhook

**Validar assinatura** (obrigatório):
```python
stripe.Webhook.construct_event(payload, sig_header, STRIPE_WEBHOOK_SECRET)
```

**Por que?**
- Garante que webhook veio realmente do Stripe
- Evita ataques fake (alguém enviando POST fake)
- Stripe assina cada webhook com secret único

**Secret do webhook**: Encontrado no Stripe Dashboard → Webhooks → [seu webhook] → Signing secret

---

## 5. SPLIT PAYMENT (TAXA DA PLATAFORMA)

### 5.1. Como Funciona

**Stripe Connect** faz split automático no momento do pagamento

**Exemplo** (produto R$ 29,90):
1. Aluno paga R$ 29,90
2. Stripe processa pagamento
3. **Split automático**:
   - Plataforma recebe: R$ 1,50 (5% de R$ 29,90)
   - Criador recebe: R$ 28,40 (95% de R$ 29,90)
4. **Taxa Stripe** (descontada depois):
   - Criador paga: R$ 1,59 (3,99% + R$ 0,39 do R$ 28,40)
   - **Criador líquido**: R$ 26,81

**Configuração no código**:
```python
session = checkout.Session.create(
    ...
    payment_intent_data={
        'application_fee_amount': int(product.price_brl * 100 * 0.05),  # 5%
        'transfer_data': {
            'destination': creator.stripe_account_id,
        },
    },
)
```

**Resultado**:
- Plataforma recebe R$ 1,50 na conta master (sem taxa Stripe)
- Criador recebe R$ 26,81 na conta dele (após taxas)

---

### 5.2. Taxa Configurável

**Plataforma pode ajustar taxa** (ex: 5% → 10%)

**Exemplo** (taxa 10%):
```python
FEE_PERCENTAGE = 0.10  # 10%

application_fee_amount = int(product.price_brl * 100 * FEE_PERCENTAGE)
```

**Comparação**:
| Taxa Plataforma | Plataforma Recebe | Criador Recebe (líquido) |
|-----------------|-------------------|--------------------------|
| 5% | R$ 1,50 | R$ 26,81 |
| 10% | R$ 2,99 | R$ 25,32 |
| 15% | R$ 4,49 | R$ 23,82 |

**Benchmark mercado**:
- Hotmart: 9,9% + R$ 0,49
- Eduzz: 10,9%
- Gumroad: 10% (até $1k), depois 3,5%
- **Recomendação**: 5-8% (competitivo)

---

## 6. PRODUTOS FÍSICOS (ENVIO)

### 6.1. Fluxo de Entrega

**Problema**: Produto físico (camiseta, livro) precisa endereço de entrega

**Solução v1** (Manual):
1. Checkout Stripe coleta endereço
2. Webhook notifica criador com dados do aluno
3. Criador envia produto manualmente (Correios, etc.)
4. Criador marca "Produto enviado" no Admin Panel

**Checkout com endereço**:
```python
session = checkout.Session.create(
    ...
    shipping_address_collection={
        'allowed_countries': ['BR'],  # Apenas Brasil v1
    },
    line_items=[{
        'price_data': {
            ...
            'product_data': {
                'name': 'Camiseta Exclusiva',
                'metadata': {'type': 'physical'},  # Indica produto físico
            },
        },
        'quantity': 1,
    }],
)
```

**Webhook extrai endereço**:
```python
if event.type == 'checkout.session.completed':
    session = event.data.object

    # Endereço de envio
    shipping = session.shipping_details
    address = {
        'name': shipping.name,
        'street': shipping.address.line1,
        'city': shipping.address.city,
        'state': shipping.address.state,
        'zip': shipping.address.postal_code,
        'country': shipping.address.country,
    }

    # Salvar endereço na compra
    purchase.shipping_address = address
    purchase.save()

    # Notificar criador
    send_email(
        to=creator.email,
        subject='Nova venda! Produto físico para enviar',
        template='creator_physical_sale',
        data={'purchase': purchase, 'address': address}
    )
```

---

### 6.2. Rastreamento de Envio (v1.1)

**v1.1**: Integração com transportadora (Correios API)

**Fluxo**:
1. Criador marca "Produto enviado" → informa código rastreio
2. Backend consulta API Correios
3. Aluno vê status de envio no Learner App
4. Email automático: "Seu produto foi enviado! Código: BR123456789"

---

## 7. REEMBOLSO

### 7.1. Fluxo de Reembolso

**Aluno pode solicitar reembolso via**:
- Stripe Dashboard (até 60 dias)
- Contato com criador (via email)

**Criador aprova reembolso** (InfoApp Admin):
1. Acessa "Histórico de Vendas"
2. Clica em compra → "Reembolsar"
3. Confirma reembolso
4. Backend chama API Stripe
5. Stripe reembolsa aluno
6. Plataforma estorna comissão (5%)

**Implementação**:
```python
@app.post("/api/admin/purchases/{purchase_id}/refund")
async def refund_purchase(purchase_id: str, creator_id: str):
    purchase = db.purchases.get(purchase_id)

    # Validar permissão (criador é dono do produto?)
    if purchase.creator_id != creator_id:
        raise HTTPException(403, "Forbidden")

    # Reembolsar via Stripe
    refund = stripe.Refund.create(
        payment_intent=purchase.stripe_payment_id,
        refund_application_fee=True,  # Estorna comissão plataforma
    )

    # Atualizar compra
    purchase.status = 'refunded'
    purchase.refunded_at = datetime.now()
    purchase.save()

    # Reverter entrega (se produto digital)
    revoke_product(purchase.user_id, purchase.product_id)

    # Notificar aluno
    send_email(
        to=purchase.user_email,
        subject='Reembolso processado',
        template='refund_success',
        data={'purchase': purchase}
    )

    return {'status': 'refunded'}
```

**Prazos Stripe**:
- Cartão: 5-10 dias úteis
- PIX: 1-2 dias úteis
- Boleto: 30 dias (limitação bancária)

---

## 8. DASHBOARD STRIPE (CRIADOR)

### 8.1. Express Dashboard

**Stripe Express Dashboard**: Criador acessa dashboard simplificado

**URL**: `https://dashboard.stripe.com/express/acct_xxxxx`

**Criador pode ver**:
- Saldo disponível
- Histórico de pagamentos
- Reembolsos
- Configurar conta bancária (receber saques)
- Ver taxas

**Link no InfoApp Admin**:
```
┌────────────────────────────────────────┐
│ 💳 Stripe                              │
│ Status: ✅ Conectado                   │
│ Saldo disponível: R$ 245,30            │
│ Próximo saque: 2025-12-30              │
│                                        │
│ [Abrir Dashboard Stripe]               │
└────────────────────────────────────────┘
```

---

## 9. SEGURANÇA E COMPLIANCE

### 9.1. PCI DSS Compliance

**Stripe cuida de tudo**:
- Plataforma NÃO armazena dados de cartão
- Stripe hospeda formulário de pagamento (Hosted Checkout)
- Plataforma apenas recebe confirmação de pagamento

**Vantagem**: Sem necessidade de certificação PCI DSS

---

### 9.2. LGPD/GDPR

**Dados armazenados**:
- Nome, email, endereço (apenas se produto físico)
- Histórico de compras
- Stripe payment_id (referência)

**Dados NÃO armazenados**:
- Número de cartão
- CVV
- Dados bancários

**Aluno pode**:
- Exportar dados (LGPD): `/api/user/export`
- Deletar conta: histórico de compras anonimizado (remove PII, mantém estatísticas)

---

## 10. TESTES

### 10.1. Modo Teste Stripe

**Stripe Test Mode**: Testar integração sem dinheiro real

**Cartões de teste**:
- `4242 4242 4242 4242`: Sucesso
- `4000 0000 0000 9995`: Cartão recusado
- Qualquer CVV: `123`
- Qualquer validade: futuro (ex: `12/30`)

**Ativar Test Mode**:
```python
stripe.api_key = 'sk_test_xxxxx'  # Chave de teste
```

**Testar fluxo completo**:
1. Criar produto no Admin (ambiente teste)
2. Comprar com cartão teste `4242...`
3. Stripe processa pagamento (fake)
4. Webhook notifica backend
5. Produto entregue

**Migrar para Produção**:
- Trocar `sk_test_xxxxx` por `sk_live_xxxxx`
- Ativar webhook em produção

---

### 10.2. Testes Automatizados

**Mockar Stripe em testes unitários**:
```python
# Python/pytest exemplo
from unittest.mock import patch

@patch('stripe.checkout.Session.create')
def test_purchase_product(mock_stripe):
    mock_stripe.return_value = MockStripeSession(url='https://checkout.stripe.com/...')

    response = client.post('/api/store/purchase/prod_123')

    assert response.status_code == 200
    assert 'checkout_url' in response.json()
```

---

## 11. PRÓXIMOS PASSOS

**v1**:
- [x] Definir integração Stripe (COMPLETO)
- [ ] Implementar OAuth Connect
- [ ] Implementar Checkout Session
- [ ] Implementar Webhooks
- [ ] Testar em Test Mode
- [ ] Migrar para Produção

**v1.1**:
- [ ] Integração transportadora (produtos físicos)
- [ ] Rastreamento de envio
- [ ] Analytics avançado (GMV, taxa conversão)

**v1.2**:
- [ ] Suporte a múltiplas moedas (USD, EUR)
- [ ] Assinaturas/Planos recorrentes (Stripe Subscriptions)
- [ ] Pagamento parcelado (Installments)

---

**Criado por**: Tech Lead + Backend Engineer
**Baseado em**: Resposta final do cliente (2025-12-27)
**Aprovado por**: Cliente ✅
**Relacionado**: `loja_hibrida.md`, `split_payment.md`
**Documentação Stripe**: https://stripe.com/docs/connect
