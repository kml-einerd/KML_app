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
💰 2.450 moedas | ⚡ 12.300 XP

[Tabs]
[Todos] [Descontos] [Premium] [Gift Cards] [Físicos]

**Destaques**
📦 Desconto 20% em Curso Avançado
    1.000 moedas
    [Comprar agora] [Comprar com Coins]

🎁 Gift Card R$50
    5.000 moedas
    [Comprar com Coins]

**Todos os Produtos (23)**
• Acesso Premium 30 dias - 2.500 moedas
  [Comprar com Coins]

• Merchandise Exclusivo - 3.000 moedas
  [Comprar com Coins]
```

## 4) Elementos & Componentes
- **WalletHeader**: Exibe saldo de moedas e XP
- **ProductCard**: Card de produto com preço em moedas
- **CTAs Duplos**: "Comprar agora" (dinheiro real) OU "Comprar com Coins" (moedas)
- **FilterTabs**: Filtrar por categoria
- **EmptyState**: "Você ainda não tem moedas suficientes. Continue aprendendo!"

[fonte: Resposta Cliente #3 → Estética Amazon/Mercado Livre simplificada, sempre 2 opções de compra]

## 5) Ação Primária
"Comprar com Coins" (usar moedas acumuladas)

## 6) Estados
- **Loading**: Skeleton de cards
- **Empty**: "Nenhum produto disponível no momento"
- **Success**: Lista completa de produtos
- **Insufficient Funds**: Botão disabled com tooltip "Você precisa de mais X moedas"

## 7) Conteúdo / Microcopy
- **Wallet**: "Você tem X moedas. Continue aprendendo para ganhar mais!"
- **Produto**: Nome claro + benefício + preço em moedas
- **Confirmação**: "Tem certeza que deseja resgatar este produto por X moedas?"
- **Sucesso**: "Produto resgatado! Confira seu email para detalhes."

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
`GET /api/learner/wallet` (saldo atual moedas/XP)
`POST /api/learner/store/purchase` (payload: `product_id`, `payment_method: coins|real`)

**Conversão XP → Moedas**: Backend define fórmula (1:1 ou custom por app)

**Integração Fulfillment**: Produtos físicos exigem endereço de entrega

[fonte: Resposta Cliente #3 → NÃO é catálogo de apps, é loja de recompensas]

## 13) Rastreabilidade
[fonte: Resposta Cliente #3 → Marketplace = Loja de Recompensas]
[fonte: Resposta Cliente #3 → Estética Amazon/Mercado Livre, 2 opções de compra]

---
