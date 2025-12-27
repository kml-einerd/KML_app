# InfoApp Admin - Configurar Loja de Recompensas

**App**: InfoApp Admin Panel
**Tela**: Configurar Loja de Recompensas
**Versão**: 2.0
**Data**: 2025-12-26
**Mudança**: MOVIDO do Modo Studio para InfoApp Admin Panel (decisão crítica do cliente)

---

## 1. CONTEXTO

**O que é**: Interface para criador configurar produtos da Loja de Recompensas do InfoApp. Alunos gastam Coins (moeda de gamificação) para comprar produtos.

**Quando**: Criador acessa via sidebar → "Loja de Recompensas"

**Usuário**: Criador (dono do InfoApp)

**Cliente disse**: "é configurado no admin do infoapp e não no modo studio e seria pelo usuário administrador"

**Estética**: Amazon/Mercado Livre simplificada

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           🏪 Loja de Recompensas                     │
│                                                                │
│                     [+ Novo Produto]  [Categorias]  [📊 Stats]│
│                     ─────────────────────────────────────────  │
│                     🔍 Buscar produtos...                      │
│                     [Todos] [Ativos] [Inativos] [Esgotados]   │
│                     ─────────────────────────────────────────  │
│                     ┌────────────┐ ┌────────────┐            │
│                     │ [📷 Img]   │ │ [📷 Img]   │            │
│                     │ Tema Escuro│ │ Certificado│            │
│                     │ 50 coins   │ │ 100 coins  │            │
│                     │ Ilimitado  │ │ 25 vendidos│            │
│                     │ ✅ Ativo    │ │ ✅ Ativo    │            │
│                     │ [Editar]   │ │ [Editar]   │            │
│                     └────────────┘ └────────────┘            │
│                     ...                                        │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Header da Tela

**Elementos**:
- Título: "Loja de Recompensas"
- Botão: "+ Novo Produto" (primário)
- Tab: "Categorias" (gerenciar categorias de produtos)
- Tab: "📊 Stats" (estatísticas de vendas)

### 3.2. Filtros e Busca

**Busca**: Campo de busca (filtra produtos por nome)

**Filtros**:
- Todos (padrão)
- Ativos (produtos visíveis na loja)
- Inativos (produtos ocultos)
- Esgotados (estoque = 0)

### 3.3. Grid de Produtos

**Layout**: Grid 3 colunas (desktop), 2 colunas (tablet), 1 coluna (mobile)

**Card de Produto**:
```
┌────────────────────┐
│ [📷 Imagem]        │
│ Tema Escuro        │
│ 50 coins           │
│ Ilimitado          │
│ ✅ Ativo            │
│ [Editar] [Duplicar]│
└────────────────────┘
```

**Informações**:
- Imagem do produto (placeholder se não tiver)
- Nome do produto
- Preço em Coins
- Estoque: "Ilimitado" ou "25 disponíveis"
- Status: Ativo (✅) ou Inativo (❌)
- Badge de tipo: "Personalização", "Desconto", "Físico", "Digital", "Power-up"

**Ações**:
- Editar → Abre modal de edição
- Duplicar → Cria cópia do produto
- Ativar/Desativar → Toggle rápido (sem modal)

### 3.4. Modal: Criar/Editar Produto

```
┌────────────────────────────────────────────────────────────────┐
│ ✕  Novo Produto                                                │
├────────────────────────────────────────────────────────────────┤
│  [Básico] [Entrega] [Configurações]                           │
│                                                                │
│  BÁSICO:                                                       │
│  Nome: [Tema Escuro Premium.................]                 │
│  Descrição: [Campo de texto longo............]                │
│  Tipo: [Dropdown: Personalização ▼]                           │
│         (Desconto, Físico, Digital, Personalização, Power-up) │
│  Preço: [50] coins                                            │
│  Imagem: [📷 Upload]  [URL da imagem atual]                   │
│                                                                │
│  Estoque:                                                      │
│  ( ) Ilimitado                                                 │
│  (●) Limitado: [100] unidades                                 │
│                                                                │
│  Status:                                                       │
│  [✅] Ativo (visível na loja)                                 │
│                                                                │
│  ─────────────────────────────────────────────────────────────│
│                                      [Cancelar]  [Salvar]     │
└────────────────────────────────────────────────────────────────┘
```

**Tabs do Modal**:
1. **Básico**: Nome, descrição, tipo, preço, imagem, estoque, status
2. **Entrega**: Como produto é entregue (depende do tipo)
3. **Configurações**: Avançado (restrições, disponibilidade por nível)

### 3.5. Tab: Entrega (depende do tipo)

**Tipo: Desconto**
```
┌────────────────────────────────────────┐
│ ENTREGA (Desconto)                     │
│ Código de cupom: [ESCURO10]           │
│ Percentual: [10]%                      │
│ Validade: [30] dias após resgate       │
│ URL de resgate: [https://...]         │
└────────────────────────────────────────┘
```

**Tipo: Físico**
```
┌────────────────────────────────────────┐
│ ENTREGA (Produto Físico)               │
│ Após compra:                           │
│ ☑️  Enviar email para criador com      │
│    dados do aluno (nome, endereço)     │
│ ☑️  Aluno preenche formulário de       │
│    endereço (CEP, rua, número...)      │
│                                        │
│ Email de notificação:                  │
│ [creator@example.com]                  │
└────────────────────────────────────────┘
```

**Tipo: Digital**
```
┌────────────────────────────────────────┐
│ ENTREGA (Produto Digital)              │
│ Após compra:                           │
│ (●) Link de download                   │
│     URL: [https://cdn.../ebook.pdf]    │
│ ( ) Código de acesso                   │
│     Código: [PREMIUM-2025]             │
│ ( ) Acesso a módulo exclusivo          │
│     Módulo: [Dropdown ▼]               │
└────────────────────────────────────────┘
```

**Tipo: Personalização**
```
┌────────────────────────────────────────┐
│ ENTREGA (Personalização)               │
│ O que é desbloqueado:                  │
│ ☑️  Tema escuro                        │
│ ☐  Avatar premium                      │
│ ☐  Efeitos sonoros personalizados     │
│ ☐  Badge especial                      │
│                                        │
│ Aplicar automaticamente após compra    │
└────────────────────────────────────────┘
```

**Tipo: Power-up**
```
┌────────────────────────────────────────┐
│ ENTREGA (Power-up)                     │
│ Tipo de power-up:                      │
│ (●) Freeze Streak (1 dia sem perder)   │
│ ( ) XP Boost 2x (24h)                  │
│ ( ) Coins Boost 2x (24h)               │
│ ( ) Pular 1 lição                      │
│                                        │
│ Duração: [24] horas                    │
│ Máximo por aluno: [5] por mês          │
└────────────────────────────────────────┘
```

### 3.6. Tab: Configurações (Avançado)

```
┌────────────────────────────────────────┐
│ CONFIGURAÇÕES                          │
│ Disponibilidade:                       │
│ ☐  Apenas para alunos nível [5]+      │
│ ☐  Apenas para alunos com streak [7]+ │
│ ☐  Disponível até [data]              │
│                                        │
│ Limite por aluno:                      │
│ [1] compra(s) por aluno (0 = ilimitado│
│                                        │
│ Categoria:                             │
│ [Dropdown: Personalização ▼]          │
└────────────────────────────────────────┘
```

### 3.7. Categorias (Tab)

**Gerenciar categorias de produtos**

```
┌────────────────────────────────────────┐
│ 🏷️  Categorias                         │
│ [+ Nova Categoria]                     │
│ ─────────────────────────────────────  │
│ 1. Personalização (3 produtos)        │
│    [Editar] [Deletar]                  │
│ 2. Descontos (5 produtos)             │
│    [Editar] [Deletar]                  │
│ 3. Power-ups (2 produtos)             │
│    [Editar] [Deletar]                  │
└────────────────────────────────────────┘
```

### 3.8. Estatísticas (Tab)

```
┌────────────────────────────────────────┐
│ 📊 Estatísticas da Loja                │
│ ─────────────────────────────────────  │
│ Total de Vendas (30 dias): 245        │
│ Coins Gastos: 12.450                   │
│ Taxa de Conversão: 18%                 │
│ ─────────────────────────────────────  │
│ Top Produtos (vendas):                 │
│ 1. Tema Escuro       85 vendas        │
│ 2. Certificado       45 vendas        │
│ 3. Freeze Streak     32 vendas        │
│ ─────────────────────────────────────  │
│ Histórico de Compras:                  │
│ [Tabela: Aluno, Produto, Data, Coins] │
└────────────────────────────────────────┘
```

---

## 4. ESTADOS

### 4.1. Empty State (sem produtos)
```
┌────────────────────────────────────────┐
│  🏪 Loja vazia                         │
│  Configure produtos para seus alunos!  │
│                                        │
│  [+ Criar Primeiro Produto]           │
│  [Ver Exemplos]                        │
└────────────────────────────────────────┘
```

### 4.2. Loading
- Skeleton cards para produtos

### 4.3. Erro
- Toast: "Erro ao salvar produto. Tente novamente."

### 4.4. Sucesso
- Toast: "Produto criado com sucesso! 🎉"
- Toast: "Produto ativado!"

---

## 5. INTERAÇÕES

### 5.1. Criar Novo Produto
1. Clique "+ Novo Produto"
2. Modal abre (tab "Básico")
3. Preenche nome, tipo, preço, imagem
4. Vai para tab "Entrega" → Configura como produto é entregue
5. (Opcional) Vai para tab "Configurações" → Define restrições
6. Clique "Salvar" → Produto criado
7. Modal fecha, produto aparece no grid

### 5.2. Editar Produto
1. Clique "Editar" no card do produto
2. Modal abre com dados preenchidos
3. Criador edita
4. Clique "Salvar" → Produto atualizado

### 5.3. Ativar/Desativar Produto
1. Clique no toggle (✅/❌) no card
2. Status muda imediatamente
3. Toast: "Produto ativado!" ou "Produto desativado!"

**Implicação**:
- Ativo: Produto visível na loja (Learner App)
- Inativo: Produto oculto (não aparece na loja)

### 5.4. Duplicar Produto
1. Clique "Duplicar"
2. Cria cópia do produto com nome "Cópia de [nome original]"
3. Status: Inativo (criador precisa ativar)

### 5.5. Ver Histórico de Compras
1. Vai para tab "📊 Stats"
2. Clique em produto no Top Produtos
3. Mostra histórico de compras daquele produto
4. Filtros: Por aluno, por data

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Preço em Coins
- Mínimo: 1 coin
- Máximo: 10.000 coins (configurável)
- Sugestão de preços:
  - Personalização: 50-200 coins
  - Power-up: 30-100 coins
  - Digital: 100-300 coins
  - Físico: 500-1000 coins
  - Desconto: 50-100 coins

### 6.2. Estoque
- Ilimitado: Não controla estoque
- Limitado: Decrementa a cada compra, produto some da loja quando estoque = 0

### 6.3. Entrega Automática vs Manual
- **Automática**: Personalização, Power-up, Digital (link/código)
  - Sistema aplica automaticamente após compra
- **Manual**: Físico, Desconto (email/formulário)
  - Criador recebe notificação e precisa entregar manualmente

### 6.4. Restrições
- Produto pode ser restrito por nível (ex: "Apenas para alunos nível 5+")
- Produto pode ter limite por aluno (ex: "Máximo 1 compra por aluno")

### 6.5. Reembolso
- v1: Não há sistema de reembolso (Coins gastos não voltam)
- v1.1: Criador pode reembolsar manualmente (creditar Coins de volta)

---

## 7. RESPONSIVO

**Desktop**: Grid 3 colunas
**Tablet**: Grid 2 colunas
**Mobile**: Grid 1 coluna, modal fullscreen

---

## 8. ANALYTICS (Tracking)

**Eventos**:
- `admin_product_created`: Ao criar produto (param: type)
- `admin_product_edited`: Ao editar produto
- `admin_product_activated`: Ao ativar produto
- `admin_product_deactivated`: Ao desativar produto
- `admin_product_duplicated`: Ao duplicar produto
- `admin_store_stats_viewed`: Ao acessar tab Stats

---

## 9. ACESSIBILIDADE

- Cards de produtos têm `aria-label` descritivo
- Modal navegável por teclado
- Imagens têm alt text

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/store/products`: Lista produtos
- `POST /api/admin/store/products`: Cria produto
- `PUT /api/admin/store/products/:id`: Atualiza produto
- `DELETE /api/admin/store/products/:id`: Deleta produto
- `GET /api/admin/store/stats`: Estatísticas da loja
- `GET /api/admin/store/purchases`: Histórico de compras

**Exemplo de payload** (criar produto):
```json
{
  "name": "Tema Escuro Premium",
  "description": "Ative o modo escuro para uma experiência visual confortável",
  "type": "customization",
  "price_coins": 50,
  "image_url": "https://cdn.../dark-theme.png",
  "stock_type": "unlimited",
  "stock_quantity": null,
  "active": true,
  "delivery_config": {
    "unlock_feature": "dark_theme",
    "auto_apply": true
  },
  "restrictions": {
    "min_level": null,
    "min_streak": null,
    "max_per_user": 1,
    "available_until": null
  },
  "category_id": "cat_customization"
}
```

**Tipos de produto** (enum):
- `discount`: Desconto/cupom
- `physical`: Produto físico (camiseta, livro)
- `digital`: Produto digital (ebook, template)
- `customization`: Personalização (tema, avatar)
- `powerup`: Power-up (freeze streak, XP boost)

---

## 11. INTEGRAÇÃO COM LEARNER APP

**Quando aluno compra produto**:
1. Aluno vai em Loja de Recompensas (Learner App)
2. Clica "Comprar" em produto
3. Modal: "Confirmar compra? -50 coins"
4. Confirma → Backend valida (aluno tem coins? produto disponível?)
5. Debita coins, cria registro de compra
6. Entrega produto:
   - **Customization/Power-up**: Aplica automaticamente
   - **Digital**: Mostra link de download
   - **Físico/Desconto**: Envia email para criador + notificação

**Notificação para criador**:
- Email: "João comprou 'Certificado Premium' por 100 coins. Endereço: ..."
- Notificação no Admin Panel (sino)

---

## 12. PERGUNTAS PARA O CLIENTE

1. **Tipos de produto prioritários v1**: Quais tipos são obrigatórios v1? (Customization, Power-up sim. Físico/Desconto talvez?)
2. **Entrega de produtos físicos**: Integração com transportadora (Correios, etc.) ou manual (criador envia por conta própria)?
3. **Reembolso**: Sistema de reembolso é necessário v1 ou v1.1?
4. **Pagamento criador → plataforma**: Criador paga comissão por venda de produtos? (ex: 10% dos Coins gastos)
5. **Produtos gratuitos**: É possível criar produto de 0 coins (grátis)?

---

**Status**: DRAFT
**Próxima revisão**: [Data]
