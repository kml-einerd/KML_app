# InfoApp Admin - Dashboard

**App**: InfoApp Admin Panel
**Tela**: Dashboard
**Versão**: 2.0
**Data**: 2025-12-26
**Mudança**: Novo componente (movido de Platform Admin para InfoApp Admin)

---

## 1. CONTEXTO

**O que é**: Tela principal do painel de administração do InfoApp. Criador vê visão geral de métricas, alunos ativos, engajamento e ações rápidas.

**Quando**: Primeira tela ao acessar `{infoapp-slug}.plataforma.com/admin`

**Usuário**: Criador (dono do InfoApp)

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Logo InfoApp]  InfoApp Admin          [Perfil] [Notifs] [?]  │
├────────────────────────────────────────────────────────────────┤
│ [Sidebar]                                                      │
│  Dashboard ●                  📊 Visão Geral - Jan 2025       │
│  Gestão Conteúdo                                               │
│  Upload Massa                  ┌──────┐ ┌──────┐ ┌──────┐    │
│  Loja Recompensas              │ 245  │ │ 182  │ │ 74%  │    │
│  Usuários                      │Alunos│ │Ativos│ │Concl.│    │
│  Analytics                     └──────┘ └──────┘ └──────┘    │
│  Configurações                                                 │
│                                ─────────────────────────────   │
│                                📈 Engajamento (últimos 7 dias) │
│                                [Gráfico de linhas]             │
│                                ─────────────────────────────   │
│                                🎯 Top Lessons (conclusão)      │
│                                1. Intro Python      95%        │
│                                2. Variáveis         89%        │
│                                3. Funções           82%        │
│                                ─────────────────────────────   │
│                                ⚡ Ações Rápidas                │
│                                [Criar Lesson] [Importar]       │
│                                [Ver Alunos]   [Configurar]     │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Header

**Elementos**:
- Logo do InfoApp (customizado)
- Nome: "InfoApp Admin"
- Botão Perfil (dropdown: Configurações, Sair)
- Notificações (sino)
- Ajuda (?)

### 3.2. Sidebar (Navegação)

**Itens**:
1. Dashboard ● (ativo)
2. Gestão de Conteúdo
3. Upload em Massa
4. Loja de Recompensas ← **NOVO** (movido do Modo Studio)
5. Usuários/Cohorts
6. Analytics
7. Configurações

**Badges**:
- "3" em Upload em Massa (3 importações pendentes)
- "!" em Configurações (ação requerida)

### 3.3. Cards de Métricas (KPIs)

**Card 1: Alunos Totais**
- Número: `245`
- Label: "Alunos"
- Tendência: +12% vs mês anterior (verde)

**Card 2: Alunos Ativos (7 dias)**
- Número: `182`
- Label: "Ativos"
- Tendência: +5% vs semana anterior

**Card 3: Taxa de Conclusão**
- Número: `74%`
- Label: "Conclusão"
- Tendência: -2% vs mês anterior (vermelho)

**Card 4: Coins Gastos**
- Número: `12.450`
- Label: "Coins Gastos (Loja)"
- Tendência: +18% vs mês anterior

### 3.4. Gráfico de Engajamento

**Tipo**: Gráfico de linhas (últimos 7 dias)

**Métricas**:
- Linha 1 (azul): Lições completadas/dia
- Linha 2 (verde): Alunos ativos/dia

**Interação**: Hover mostra tooltip com valores exatos

### 3.5. Lista: Top Lessons

**Ordenação**: Por taxa de conclusão (desc)

**Colunas**:
- Lesson (nome)
- Taxa de conclusão (%)
- Ação: [Ver Detalhes]

**Limite**: Top 5 lessons

### 3.6. Ações Rápidas

**Botões**:
1. **Criar Lesson** → Redireciona para Gestão de Conteúdo
2. **Importar Conteúdo** → Redireciona para Upload em Massa
3. **Ver Alunos** → Redireciona para Usuários
4. **Configurar Loja** → Redireciona para Loja de Recompensas

---

## 4. ESTADOS

### 4.1. Loading
- Skeleton cards enquanto carrega métricas
- Spinner no gráfico

### 4.2. Empty State (InfoApp sem alunos)
```
┌────────────────────────────────────────┐
│  📚 Seu InfoApp está pronto!           │
│  Nenhum aluno cadastrado ainda.        │
│                                        │
│  [Convidar Alunos]  [Compartilhar]    │
└────────────────────────────────────────┘
```

### 4.3. Erro (falha ao carregar métricas)
- Toast: "Erro ao carregar dashboard. Tente novamente."

---

## 5. INTERAÇÕES

### 5.1. Ações de Header
- Clique em Notificações → Modal com lista de notificações
- Clique em Perfil → Dropdown (Configurações, Sair)

### 5.2. Clique em Card de Métrica
- Redireciona para Analytics (filtrado pela métrica)
- Ex: Clique em "Alunos Ativos" → Analytics com filtro "7 dias"

### 5.3. Clique em Top Lesson
- Redireciona para Gestão de Conteúdo → Editar Lesson

### 5.4. Ações Rápidas
- Clique em "Criar Lesson" → Modal ou página de criação

---

## 6. RESPONSIVO

**Desktop (> 1024px)**: Layout com sidebar + conteúdo
**Tablet (768-1024px)**: Sidebar colapsada (ícones)
**Mobile (< 768px)**: Sidebar vira menu hambúrguer, cards em coluna única

---

## 7. ANALYTICS (Tracking)

**Eventos**:
- `admin_dashboard_viewed`: Ao carregar dashboard
- `admin_quick_action_clicked`: Ao clicar em ação rápida (param: action)
- `admin_metric_clicked`: Ao clicar em card de métrica (param: metric)

---

## 8. ACESSIBILIDADE

- Cards têm `aria-label` descritivo
- Gráfico tem tabela de dados oculta para screen readers
- Navegação por teclado (Tab) funciona corretamente

---

## 9. NOTAS TÉCNICAS

**API Endpoint**: `GET /api/admin/dashboard`

**Resposta**:
```json
{
  "metrics": {
    "total_learners": 245,
    "active_learners_7d": 182,
    "completion_rate": 74.2,
    "coins_spent_30d": 12450
  },
  "engagement_chart": {
    "labels": ["Jan 20", "Jan 21", ...],
    "lessons_completed": [12, 15, 10, ...],
    "active_learners": [45, 50, 42, ...]
  },
  "top_lessons": [
    {"id": "l1", "title": "Intro Python", "completion_rate": 95},
    ...
  ]
}
```

**Cache**: 5 minutos (refetch automático)

---

**Status**: DRAFT
**Próxima revisão**: [Data]
