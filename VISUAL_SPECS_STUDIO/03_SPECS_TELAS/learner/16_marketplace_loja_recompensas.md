# MARKETPLACE / LOJA DE RECOMPENSAS

## 1) Objetivo da Tela
Permitir que aluno troque XP/moedas por produtos e benefícios reais.
[fonte: Resposta Cliente #3 → Marketplace é LOJA DE RECOMPENSAS onde aluno troca moedas/XP por produtos/benefícios]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer resgatar moedas acumuladas, **Frequência**: 1-2x por semana

## 3) Layout & Hierarquia
```
[Header]
**Minha Carteira**
💰 2.450 Coins (saldo) | 📊 8.500 Coins (lifetime)

[Tabs]
[Todos] [Descontos] [Premium] [Gift Cards] [Físicos]

**Destaques**
📦 Certificado Premium
    500 Coins OU R$ 29,90
    [Comprar com Coins] [Comprar por R$ 29,90]

🎁 Tema Escuro
    50 Coins
    [Comprar com Coins]

**Todos os Produtos (23)**
• Acesso Premium 30 dias - 1.000 Coins OU R$ 49,00
  [Comprar com Coins] [Comprar por R$]

• Camiseta Exclusiva - R$ 79,90
  [Comprar por R$ 79,90]

• Avatar Premium - 100 Coins
  [Comprar com Coins]
```

## 4) Elementos & Componentes
- **WalletHeader**: Exibe saldo de Coins (disponível) e Coins Lifetime (acumulado total)
- **ProductCard**: Card de produto com preço em Coins, Reais, ou AMBOS
- **CTAs Híbridos**:
  - "Comprar com Coins" (gamificação, se produto aceita Coins)
  - "Comprar por R$ X" (pagamento real via Stripe, se produto aceita Reais)
  - Se produto aceita AMBOS → mostra 2 botões lado a lado
- **FilterTabs**: Filtrar por categoria
- **EmptyState**: "Você ainda não tem Coins suficientes. Continue estudando para ganhar mais!"

[fonte: Resposta Cliente FINAL → Loja aceita Coins + Reais (criador cobra por produtos)]

## 5) Ação Primária
"Comprar com Coins" (usar moedas acumuladas)

## 6) Estados
- **Loading**: Skeleton de cards
- **Empty**: "Nenhum produto disponível no momento"
- **Success**: Lista completa de produtos
- **Insufficient Funds**: Botão disabled com tooltip "Você precisa de mais X moedas"

## 7) Conteúdo / Microcopy
- **Wallet**: "Você tem X Coins disponíveis. Continue estudando para ganhar mais!"
- **Produto Apenas Coins**: "50 Coins"
- **Produto Apenas Reais**: "R$ 29,90"
- **Produto Híbrido**: "500 Coins OU R$ 29,90"
- **Confirmação (Coins)**: "Confirmar compra? -500 coins (você ficará com X coins)"
- **Confirmação (Reais)**: Redireciona para Stripe Checkout
- **Sucesso (Coins)**: "Produto comprado com Coins! 🎉"
- **Sucesso (Reais)**: "Pagamento confirmado! Produto enviado para seu email."
- **Saldo Insuficiente**: "Você precisa de 500 Coins mas tem apenas 245. Faltam 255 coins!"

## 8) Som/Haptics
**STATUS** (recompensa/conquista): `purchase_success.mp3`, `coins_spend.mp3`

## 9) Eventos
`store_viewed`, `product_browsed`, `product_purchased`, `insufficient_funds`

## 10) Definition of Done
- [ ] WalletHeader exibe saldo correto
- [ ] Produtos listados com preço em moedas
- [ ] Filtros por categoria funcionam
- [ ] "Comprar com Coins" valida saldo
- [ ] Confirmação antes de compra
- [ ] Sucesso atualiza saldo
- [ ] EmptyState quando sem moedas

## 11) Modo Studio / Edições
**MicroSaaS**: Loja desabilitada (feature Standard+)
**Standard/Full**: Loja habilitada

**Configuração Creator**: Creator pode ativar/desativar loja no seu app via tela Rewards/Economy

## 12) Mapeamento Back
`GET /api/learner/store/products` (lista produtos disponíveis)
  → Response: `{ products: [{ id, name, price_coins, price_brl, accepts_coins, accepts_brl, ... }] }`

`GET /api/learner/wallet` (saldo atual Coins)
  → Response: `{ coins_balance, coins_lifetime, streak_days }`

`POST /api/learner/store/purchase` (payload: `product_id`, `payment_method: coins|brl`)
  → Se `payment_method: coins` → Debita Coins e entrega produto
  → Se `payment_method: brl` → Retorna `checkout_url` (Stripe Checkout)

`GET /api/learner/store/checkout/success?session_id={stripe_session_id}`
  → Confirma pagamento Stripe e entrega produto

**Integração Stripe**: Produtos com preço em Reais requerem Stripe Connect configurado pelo criador

**Integração Fulfillment**: Produtos físicos coletam endereço no Stripe Checkout

[fonte: Resposta Cliente FINAL → Loja híbrida Coins + Reais]

## 13) Rastreabilidade
[fonte: Resposta Cliente #3 → Marketplace = Loja de Recompensas]
[fonte: Resposta Cliente #3 → Estética Amazon/Mercado Livre, 2 opções de compra]

---
