# Split Payment (Taxa da Plataforma)

**Versão**: 1.0 (NOVO)
**Data**: 2025-12-27
**Status**: OFICIAL (baseado em resposta final do cliente)
**Mudança Crítica**: Plataforma cobra taxa opcional sobre vendas em Reais

---

## CONTEXTO

**Cliente disse**: ✅ "Loja aceita Coins + Reais (criador cobra por produtos)"

**DECISÃO DE MONETIZAÇÃO**:
- Produtos pagos com **Coins**: 0% taxa (100% gamificação)
- Produtos pagos com **Reais**: Taxa configurável (ex: 5%) via Stripe Connect
- Criador recebe pagamento descontando taxa da plataforma + taxa Stripe

---

## 1. VISÃO GERAL

### 1.1. Modelo de Split Payment

**Split Payment**: Divisão automática do pagamento entre plataforma e criador

**Exemplo** (Certificado R$ 29,90):
```
Aluno paga: R$ 29,90
    ↓
Stripe processa
    ↓
Split automático:
├─ Plataforma recebe: R$ 1,50 (5%)
└─ Criador recebe: R$ 28,40 (95%)
    ↓
Stripe cobra taxa do criador:
├─ Taxa Stripe: R$ 1,59 (3,99% + R$ 0,39)
└─ Criador líquido: R$ 26,81
```

**Fluxo de dinheiro**:
1. Aluno → Stripe: R$ 29,90
2. Stripe → Plataforma: R$ 1,50 (5% fee)
3. Stripe → Criador: R$ 26,81 (95% - taxa Stripe)

---

### 1.2. Por que Cobrar Taxa?

**Custos da Plataforma**:
- Hospedagem (Cloud Run, Cloud SQL)
- TTS (ElevenLabs)
- Storage (imagens, arquivos)
- Stripe Connect (sem custo fixo, mas implica manutenção)
- Suporte técnico
- Desenvolvimento contínuo

**Benchmark mercado**:
| Plataforma | Taxa | Observações |
|------------|------|-------------|
| Hotmart | 9,9% + R$ 0,49 | Marketplace cursos |
| Eduzz | 10,9% | Marketplace digital |
| Gumroad | 10% (até $1k), depois 3,5% | Produtos digitais |
| Shopify | R$ 79/mês + 2% | E-commerce |
| **Nossa Plataforma** | **5-8%** | Competitivo |

**Recomendação**: 5% (v1) → Ajustar baseado em tração

---

## 2. IMPLEMENTAÇÃO TÉCNICA (STRIPE CONNECT)

### 2.1. Application Fee (Stripe)

**Stripe Connect**: `application_fee_amount` define taxa da plataforma

```python
# Backend (Python/FastAPI exemplo)
import stripe

session = stripe.checkout.Session.create(
    payment_method_types=['card', 'boleto'],
    line_items=[{
        'price_data': {
            'currency': 'brl',
            'product_data': {'name': 'Certificado Premium'},
            'unit_amount': 2990,  # R$ 29,90 em centavos
        },
        'quantity': 1,
    }],
    mode='payment',
    success_url='https://plataforma.com/success',
    cancel_url='https://plataforma.com/cancel',

    # SPLIT PAYMENT
    payment_intent_data={
        'application_fee_amount': 150,  # R$ 1,50 em centavos (5%)
        'transfer_data': {
            'destination': 'acct_creator_xxxxx',  # Conta Stripe do criador
        },
    },
)
```

**Resultado**:
- Stripe transfere R$ 1,50 para plataforma (Application Fee)
- Stripe transfere R$ 28,40 para criador (depois desconta taxa Stripe)
- Criador recebe líquido: R$ 26,81

---

### 2.2. Cálculo Dinâmico da Taxa

**Taxa configurável** (ex: 5%, 8%, 10%)

```python
# Config global (environment variable ou DB)
PLATFORM_FEE_PERCENTAGE = 0.05  # 5%

def calculate_application_fee(price_brl: float) -> int:
    """
    Calcula application fee em centavos
    Args:
        price_brl: Preço em reais (float)
    Returns:
        Application fee em centavos (int)
    """
    fee_brl = price_brl * PLATFORM_FEE_PERCENTAGE
    fee_cents = int(fee_brl * 100)
    return fee_cents

# Exemplo
price = 29.90
fee = calculate_application_fee(price)  # 150 (R$ 1,50)
```

**Taxa diferenciada por criador** (v1.1):
```python
# DB
CREATE TABLE creator_fees (
  creator_id UUID PRIMARY KEY,
  fee_percentage DECIMAL(5,2) DEFAULT 5.00,  -- 5%
  created_at TIMESTAMP DEFAULT NOW()
);

# Backend
def get_creator_fee(creator_id):
    fee = db.creator_fees.get(creator_id)
    return fee.fee_percentage if fee else 0.05  # Default 5%

# Uso
creator_fee = get_creator_fee(creator.id)
application_fee = int(product.price_brl * 100 * creator_fee)
```

**Casos de uso**:
- Criador VIP: 3% (incentivo)
- Criador novo: 5% (padrão)
- Criador alto volume: 2% (desconto por escala)

---

## 3. TRANSPARÊNCIA E COMUNICAÇÃO

### 3.1. Mostrar Taxa para Criador

**InfoApp Admin: Configurar Produto**
```
┌────────────────────────────────────────┐
│ PRECIFICAÇÃO                           │
│ Preço em Reais: [29.90] BRL           │
│                                        │
│ 📊 Estimativa de Recebimento:          │
│                                        │
│ Aluno paga:           R$ 29,90         │
│ Taxa plataforma (5%): -R$ 1,50         │
│ Taxa Stripe (4,39%):  -R$ 1,59         │
│ ────────────────────────────────────   │
│ Você recebe:          R$ 26,81 (89,7%) │
│                                        │
│ 💡 Dica: Ajuste o preço considerando   │
│    as taxas para alcançar o valor      │
│    desejado de recebimento             │
└────────────────────────────────────────┘
```

**Calculadora interativa**:
- Criador digita "Quero receber R$ 25,00"
- Sistema calcula preço necessário: R$ 28,16
- Sistema sugere: "Defina preço de R$ 29,90 para receber ~R$ 26,81"

---

### 3.2. Mostrar Taxa para Aluno (Opcional)

**Debate**: Mostrar ou não breakdown de taxas para aluno?

**Opção A: Não mostrar** (recomendado v1)
- Aluno vê apenas: "R$ 29,90"
- Simplifica UX
- Padrão do mercado (Amazon, Hotmart, etc.)

**Opção B: Mostrar breakdown** (v1.1 opcional)
```
┌────────────────────────────────────────┐
│ Certificado Premium                    │
│                                        │
│ Subtotal:          R$ 26,81            │
│ Taxa plataforma:   R$ 1,50             │
│ Taxa processamento:R$ 1,59             │
│ ────────────────────────────────────   │
│ Total:             R$ 29,90            │
└────────────────────────────────────────┘
```

**Recomendação**: Não mostrar breakdown (v1). Apenas "Total: R$ 29,90"

---

## 4. RECEITA DA PLATAFORMA

### 4.1. Projeção de Receita

**Cenário v1** (6 meses):
- 10 criadores ativos
- 5 criadores monetizando (50%)
- Média: 20 vendas/mês por criador
- Ticket médio: R$ 30,00
- Taxa: 5%

**Cálculo**:
```
Vendas/mês: 5 criadores × 20 vendas = 100 vendas
GMV/mês: 100 × R$ 30 = R$ 3.000
Receita plataforma (5%): R$ 150/mês
```

**Cenário v1.1** (12 meses):
- 50 criadores ativos
- 25 criadores monetizando (50%)
- Média: 30 vendas/mês por criador
- Ticket médio: R$ 35,00
- Taxa: 5%

**Cálculo**:
```
Vendas/mês: 25 criadores × 30 vendas = 750 vendas
GMV/mês: 750 × R$ 35 = R$ 26.250
Receita plataforma (5%): R$ 1.312,50/mês
```

**Break-even** (custos mensais):
- Hospedagem GCP: ~R$ 300/mês
- TTS (100 criadores): ~R$ 200/mês
- **Total**: ~R$ 500/mês

**Break-even vendas**:
```
Receita necessária: R$ 500
Taxa: 5%
GMV necessário: R$ 500 / 0,05 = R$ 10.000/mês
```

**Conclusão**: Plataforma atinge break-even com ~R$ 10k GMV/mês

---

### 4.2. Dashboard de Receita (Platform Admin)

**Tela: Analytics de Receita**
```
┌────────────────────────────────────────┐
│ 💰 Receita da Plataforma               │
├────────────────────────────────────────┤
│ MÊS ATUAL (Dezembro 2025)              │
│                                        │
│ GMV (Gross Merch. Value): R$ 26.250    │
│ Receita plataforma (5%):  R$ 1.312,50  │
│ Vendas totais:            750          │
│ Ticket médio:             R$ 35,00     │
│                                        │
│ ────────────────────────────────────   │
│                                        │
│ TOP CRIADORES (GMV)                    │
│ 1. Criador A  R$ 5.200  (160 vendas)   │
│ 2. Criador B  R$ 3.800  (95 vendas)    │
│ 3. Criador C  R$ 2.100  (58 vendas)    │
│                                        │
│ ────────────────────────────────────   │
│                                        │
│ GRÁFICO DE RECEITA (últimos 6 meses)   │
│ [Bar chart visual]                     │
└────────────────────────────────────────┘
```

**Métricas importantes**:
- GMV (Gross Merchandise Value): Total vendido
- Receita plataforma: GMV × Taxa
- Take rate: % da plataforma sobre GMV
- Vendas por criador
- Crescimento MoM (month-over-month)

---

## 5. EDGE CASES E EXCEÇÕES

### 5.1. Reembolso (Estornar Taxa)

**Problema**: Aluno pede reembolso. Plataforma deve devolver taxa?

**Solução**: Sim (obrigatório Stripe Connect)

**Implementação**:
```python
refund = stripe.Refund.create(
    payment_intent=purchase.stripe_payment_id,
    refund_application_fee=True,  # Estorna taxa plataforma
    reverse_transfer=True,        # Reverte transfer para criador
)
```

**Resultado**:
- Stripe reembolsa R$ 29,90 para aluno
- Stripe estorna R$ 1,50 da plataforma
- Stripe estorna R$ 28,40 do criador
- Todos voltam ao estado inicial

---

### 5.2. Produto Gratuito (Taxa 0%)

**Cenário**: Criador quer vender produto por R$ 0,00 (grátis)?

**Solução**: Não permitir R$ 0,00 em checkout Stripe
- Stripe não processa pagamentos de R$ 0
- Se produto é grátis → usar **apenas Coins** (0 coins = grátis)

**Validação**:
```python
if product.price_brl is not None and product.price_brl < 1.00:
    raise ValueError("Preço mínimo: R$ 1,00")
```

---

### 5.3. Criador Remove Stripe (Após Vendas)

**Cenário**: Criador desconecta Stripe após já ter vendido produtos

**Problema**: Alunos podem pedir reembolso, mas criador não tem Stripe conectado

**Solução**:
- Não permitir desconectar Stripe se houver vendas ativas
- Se criador desconecta → bloquear novos produtos com Reais
- Vendas antigas continuam reembolsáveis via Stripe (até 90 dias)

**Validação**:
```python
def disconnect_stripe(creator_id):
    sales_count = db.purchases.count_active(creator_id, payment_method='brl')

    if sales_count > 0:
        raise HTTPException(
            400,
            "Não é possível desconectar Stripe. Você tem vendas ativas que podem ser reembolsadas."
        )

    # Desconectar
    db.creators.update(creator_id, stripe_account_id=None)
```

---

## 6. CONFIGURAÇÃO AVANÇADA (v1.1)

### 6.1. Taxa por Tipo de Produto

**Cenário**: Plataforma quer cobrar taxas diferentes por tipo de produto

**Exemplo**:
- Digital: 5%
- Físico: 8% (custos logísticos)
- Desconto: 3% (baixo valor agregado)

**Implementação**:
```python
FEES_BY_TYPE = {
    'digital': 0.05,
    'physical': 0.08,
    'discount': 0.03,
    'customization': 0.05,
    'powerup': 0.05,
}

def calculate_application_fee(product):
    fee_percentage = FEES_BY_TYPE.get(product.type, 0.05)
    return int(product.price_brl * 100 * fee_percentage)
```

---

### 6.2. Taxa Progressiva (Volume)

**Cenário**: Criador com alto volume paga taxa menor

**Exemplo**:
- GMV < R$ 1.000/mês: 5%
- GMV R$ 1.000-5.000/mês: 4%
- GMV > R$ 5.000/mês: 3%

**Implementação**:
```python
def get_creator_fee_tier(creator_id):
    gmv_current_month = db.purchases.get_gmv_month(creator_id)

    if gmv_current_month < 1000:
        return 0.05  # 5%
    elif gmv_current_month < 5000:
        return 0.04  # 4%
    else:
        return 0.03  # 3%
```

**Recalcula mensalmente** (no início do mês)

---

## 7. COMPLIANCE E TRANSPARÊNCIA

### 7.1. Termos de Serviço

**Documento obrigatório**: Termos de Serviço (ToS) para criadores

**Seção: Taxas e Pagamentos**
```
6. TAXAS E PAGAMENTOS

6.1. Taxa da Plataforma
A Plataforma cobra uma taxa de 5% sobre cada venda realizada
por meio de pagamento em Reais (BRL). Vendas pagas com Coins
(gamificação) não têm taxa.

6.2. Taxas de Terceiros
Além da taxa da Plataforma, o Criador está sujeito às taxas
do Stripe (processador de pagamentos):
- Cartão: 3,99% + R$ 0,39 por transação
- PIX: 0,99%
- Boleto: R$ 2,99 por transação

6.3. Recebimento
O Criador receberá o valor líquido (após taxas) em sua conta
Stripe em até 7 dias úteis após a confirmação do pagamento.

6.4. Reembolsos
Em caso de reembolso solicitado pelo aluno, a taxa da Plataforma
será estornada proporcionalmente.
```

---

### 7.2. Nota Fiscal (Brasil)

**Obrigação fiscal**: Plataforma precisa emitir nota fiscal sobre taxa recebida

**Cenário**:
- Plataforma recebe R$ 1.312,50 (taxa 5% de R$ 26.250 GMV)
- Precisa emitir nota fiscal de **serviço de intermediação**
- Paga ISS (Imposto sobre Serviços): ~2-5% dependendo do município

**Solução v1**:
- Usar serviço de contador (ex: Contabilizei, Qipu)
- Emissão manual mensal (até volume baixo)

**Solução v1.1**:
- Integração automática (NF-e API)
- Emite nota automaticamente ao final do mês

---

## 8. MÉTRICAS E ACOMPANHAMENTO

### 8.1. KPIs Importantes

**Para Plataforma**:
- GMV (Gross Merchandise Value): Total vendido
- Receita plataforma: GMV × Taxa
- Take rate: % da plataforma sobre GMV
- Número de criadores monetizando
- Ticket médio por venda
- Crescimento MoM

**Para Criador**:
- Vendas totais (mês)
- GMV (mês)
- Receita líquida (após taxas)
- Taxa de conversão (visitantes → compras)
- Produtos mais vendidos

---

### 8.2. Alertas e Notificações

**Criador**:
- "Você vendeu R$ 1.000 este mês! Taxa reduzida para 4%"
- "Nova venda: Certificado Premium por R$ 29,90 (você recebe R$ 26,81)"

**Plataforma**:
- "GMV atingiu R$ 10k/mês (break-even)!"
- "Criador A atingiu R$ 5k GMV (considerar desconto em taxa)"

---

## 9. PRÓXIMOS PASSOS

**v1**:
- [x] Definir modelo de split payment (COMPLETO)
- [ ] Implementar taxa fixa 5%
- [ ] Adicionar calculadora de recebimento (Admin)
- [ ] Implementar reembolso com estorno de taxa
- [ ] Adicionar dashboard de receita (Platform Admin)

**v1.1**:
- [ ] Taxa diferenciada por criador
- [ ] Taxa progressiva por volume
- [ ] Taxa por tipo de produto
- [ ] Integração NF-e automática

**v2**:
- [ ] Programa de afiliados (criador indica criador → desconto em taxa)
- [ ] Planos de criador (paga mensalidade → taxa menor)

---

**Criado por**: Product Lead + Finance Analyst
**Baseado em**: Resposta final do cliente (2025-12-27)
**Aprovado por**: Cliente ✅
**Relacionado**: `loja_hibrida.md`, `stripe_integration.md`
