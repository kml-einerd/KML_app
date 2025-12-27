# LOJA DE RECOMPENSAS (CONFIGURAÇÃO GLOBAL)

## 1) Objetivo: Configurar catálogo global de produtos/benefícios que alunos podem trocar por XP/moedas.
[fonte: Resposta Cliente #3 → Marketplace é LOJA DE RECOMPENSAS, não catálogo de apps]

## 2) Usuário: Platform Admin, **Contexto**: Configuração de produtos da loja

## 3) Layout
```
**Catálogo de Produtos da Loja (23 produtos)**

[Search] [Filtros: Categoria/Status]

**Produtos Ativos**
• Desconto 20% em Curso Avançado - 1000 moedas
  [Editar] [Desativar] [Ver Resgates]

• Acesso Premium 30 dias - 2500 moedas
  [Editar] [Desativar]

• Gift Card R$50 - 5000 moedas
  [Editar] [Desativar]

**Categorias**
- Descontos em cursos
- Acesso premium temporário
- Merchandise físico
- Gift cards
- Benefícios exclusivos
```

## 4) CTA Primária: "Adicionar Produto"
## 5) Estados: Loading, Empty ("Nenhum produto cadastrado"), Success
## 6) Som: **SAFE**
## 7) Eventos: `store_catalog_viewed`, `product_created`, `product_edited`
## 8) DoD: [ ] Lista de produtos, [ ] Criar/editar produtos, [ ] Categorias funcionam, [ ] Integração com conversão XP→Moedas
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/store/products`, `POST /api/admin/store/products`, `PUT /api/admin/store/products/:id`

**IMPORTANTE**: Loja de recompensas permite alunos trocarem XP/moedas por produtos/benefícios.
**NÃO** é catálogo de apps criados (discovery de InfoApps).

[fonte: Resposta Cliente #3 → Estética Amazon/Mercado Livre simplificada, 2 opções: "Comprar agora" ou "Comprar com Coins"]
---
