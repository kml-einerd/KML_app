# InfoApp Admin - Analytics

**App**: InfoApp Admin Panel
**Tela**: Analytics (Métricas e Relatórios)
**Versão**: 2.0
**Data**: 2025-12-26

---

## 1. CONTEXTO

**O que é**: Dashboard de analytics para criador visualizar métricas detalhadas de engajamento, conclusão, retenção e desempenho do InfoApp.

**Quando**: Criador acessa via sidebar → "Analytics"

**Usuário**: Criador (dono do InfoApp)

**Filosofia**: Analytics Light (v1) - métricas essenciais sem complexidade excessiva

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           📊 Analytics                               │
│                                                                │
│                     [Visão Geral] [Engajamento] [Lições]      │
│                     [Gamificação] [Loja] [Exportar]           │
│                     ─────────────────────────────────────────  │
│                     📅 Período: [Últimos 30 dias ▼]           │
│                     ─────────────────────────────────────────  │
│                     KPIs                                       │
│                     ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐      │
│                     │ 245  │ │ 182  │ │ 74%  │ │ 42%  │      │
│                     │Alunos│ │Ativos│ │Concl.│ │Reten.│      │
│                     │+12%  │ │+5%   │ │-2%   │ │+8%   │      │
│                     └──────┘ └──────┘ └──────┘ └──────┘      │
│                     ─────────────────────────────────────────  │
│                     📈 Alunos Ativos (últimos 30 dias)         │
│                     [Gráfico de linhas]                        │
│                     ─────────────────────────────────────────  │
│                     🎯 Funil de Aprendizado                    │
│                     [Gráfico de funil]                         │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Tabs de Categorias

**Tabs**:
1. **Visão Geral** (padrão): KPIs principais + gráficos resumidos
2. **Engajamento**: Atividade diária, sessões, tempo médio
3. **Lições**: Conclusão por lesson, heatmap de dificuldade
4. **Gamificação**: XP, Coins, Streaks, Badges
5. **Loja**: Vendas de produtos, receita em Coins
6. **Exportar**: Baixar relatórios em CSV/PDF

### 3.2. Seletor de Período

**Dropdown**: Período de análise
- Últimos 7 dias
- Últimos 30 dias (padrão)
- Últimos 90 dias
- Todo o tempo
- Custom (date picker)

**Comparação**: Checkbox "Comparar com período anterior"
- Mostra tendência (+12%, -5%, etc.) em relação ao período anterior

### 3.3. KPIs (Cartões de Métrica)

**Card 1: Total de Alunos**
- Número: `245`
- Tendência: `+12%` vs período anterior (verde)
- Gráfico sparkline (mini gráfico de linha)

**Card 2: Alunos Ativos**
- Número: `182` (alunos que logaram no período)
- Tendência: `+5%`
- % do total: `74%`

**Card 3: Taxa de Conclusão**
- Número: `74%` (média de conclusão de lessons)
- Tendência: `-2%` (vermelho)

**Card 4: Taxa de Retenção**
- Número: `42%` (alunos que voltaram após 1ª sessão)
- Tendência: `+8%`

### 3.4. Gráficos - Visão Geral

**Gráfico 1: Alunos Ativos (últimos 30 dias)**
- Tipo: Linha
- Eixo X: Data
- Eixo Y: Alunos ativos/dia
- Tooltip: Hover mostra valor exato

**Gráfico 2: Funil de Aprendizado**
- Tipo: Funil
- Etapas:
  1. Signups: 300
  2. Completaram onboarding: 245 (82%)
  3. Completaram 1ª lesson: 200 (67%)
  4. Completaram 5+ lessons: 150 (50%)
  5. Completaram InfoApp: 100 (33%)

**Gráfico 3: Distribuição de Níveis**
- Tipo: Barras
- Eixo X: Nível (1, 2, 3...)
- Eixo Y: Quantidade de alunos

---

### 3.5. Tab: Engajamento

```
┌────────────────────────────────────────┐
│ 📊 Engajamento                         │
│ ─────────────────────────────────────  │
│ Sessões Totais: 1.245                  │
│ Sessões/aluno (média): 5.1             │
│ Tempo médio/sessão: 18 min             │
│ Taxa de abandono: 15%                  │
│ ─────────────────────────────────────  │
│ 📈 Sessões por Dia                     │
│ [Gráfico de barras]                    │
│ ─────────────────────────────────────  │
│ ⏱️  Tempo de Uso por Dia da Semana     │
│ [Heatmap: Seg-Dom, 00h-23h]            │
│ (mostra quando alunos mais estudam)    │
└────────────────────────────────────────┘
```

**Métricas**:
- Sessões totais (count de logins)
- Sessões/aluno (média)
- Tempo médio por sessão
- Taxa de abandono (alunos que não voltaram após 1ª sessão)

**Gráficos**:
- Sessões por dia (barras)
- Heatmap de uso (dia da semana x hora do dia)

---

### 3.6. Tab: Lições

```
┌────────────────────────────────────────┐
│ 📚 Lições                              │
│ ─────────────────────────────────────  │
│ 🏆 Top Lessons (conclusão)             │
│ 1. Intro Python       95% (200/210)    │
│ 2. Variáveis          89% (185/210)    │
│ 3. Funções            82% (172/210)    │
│ ─────────────────────────────────────  │
│ ⚠️  Lessons com Baixa Conclusão        │
│ 1. Loops              45% (95/210)     │
│ 2. POO                38% (80/210)     │
│ ─────────────────────────────────────  │
│ 🔥 Heatmap de Dificuldade              │
│ [Heatmap: Lesson x Checkpoint]         │
│ (mostra checkpoints com mais erros)    │
└────────────────────────────────────────┘
```

**Métricas**:
- Taxa de conclusão por lesson
- Taxa de acerto por checkpoint
- Tempo médio para completar lesson

**Gráficos**:
- Heatmap de dificuldade (checkpoints com mais erros ficam vermelho)
- Gráfico de barras: Conclusão por lesson

**Insight**:
- "⚠️  Lesson 'Loops' tem 45% conclusão. Revisar conteúdo?"

---

### 3.7. Tab: Gamificação

```
┌────────────────────────────────────────┐
│ 🎮 Gamificação                         │
│ ─────────────────────────────────────  │
│ XP Total Gerado: 304.250               │
│ Coins Total Gerado: 60.850             │
│ Coins Gasto: 12.450 (20%)              │
│ ─────────────────────────────────────  │
│ 📊 Distribuição de XP                  │
│ [Gráfico de pizza: fonte de XP]       │
│ - Lessons: 60%                         │
│ - Checkpoints: 25%                     │
│ - Streaks: 10%                         │
│ - Badges: 5%                           │
│ ─────────────────────────────────────  │
│ 🔥 Streaks                             │
│ Streak médio: 5.2 dias                 │
│ Longest streak: 45 dias (João Silva)  │
│ Alunos com streak 7+: 85 (35%)         │
│ ─────────────────────────────────────  │
│ 🏆 Badges Mais Ganhos                  │
│ 1. 🌱 Iniciante       245 (100%)       │
│ 2. 🔥 Aprendiz        150 (61%)        │
│ 3. ⭐ Dedicado        85 (35%)         │
└────────────────────────────────────────┘
```

---

### 3.8. Tab: Loja

```
┌────────────────────────────────────────┐
│ 🏪 Loja de Recompensas                 │
│ ─────────────────────────────────────  │
│ Vendas Totais: 245                     │
│ Coins Gastos: 12.450                   │
│ Taxa de Conversão: 18% (alunos que     │
│                    compraram algo)     │
│ ─────────────────────────────────────  │
│ 🏆 Top Produtos (vendas)               │
│ 1. Tema Escuro       85 vendas (4.250c)│
│ 2. Certificado       45 vendas (4.500c)│
│ 3. Freeze Streak     32 vendas (960c)  │
│ ─────────────────────────────────────  │
│ 📈 Vendas por Dia                      │
│ [Gráfico de linhas]                    │
│ ─────────────────────────────────────  │
│ 💰 Receita por Tipo de Produto         │
│ [Gráfico de pizza]                     │
│ - Personalização: 45%                  │
│ - Digital: 30%                         │
│ - Power-up: 15%                        │
│ - Desconto: 10%                        │
└────────────────────────────────────────┘
```

---

### 3.9. Tab: Exportar

```
┌────────────────────────────────────────┐
│ 📥 Exportar Relatórios                 │
│ ─────────────────────────────────────  │
│ Selecione o relatório:                 │
│ [Dropdown: Visão Geral ▼]             │
│   - Visão Geral                        │
│   - Engajamento                        │
│   - Lições                             │
│   - Gamificação                        │
│   - Loja                               │
│   - Relatório Completo                 │
│                                        │
│ Formato:                               │
│ (●) CSV                                │
│ ( ) PDF                                │
│ ( ) JSON                               │
│                                        │
│ Período:                               │
│ [Últimos 30 dias ▼]                   │
│                                        │
│ [Exportar]                             │
└────────────────────────────────────────┘
```

---

## 4. ESTADOS

### 4.1. Loading
- Skeleton para gráficos e KPIs

### 4.2. Empty State (sem dados)
```
┌────────────────────────────────────────┐
│  📊 Sem dados suficientes              │
│  Aguarde atividade de alunos para ver  │
│  analytics.                            │
└────────────────────────────────────────┘
```

### 4.3. Erro
- Toast: "Erro ao carregar analytics. Tente novamente."

---

## 5. INTERAÇÕES

### 5.1. Mudar Período
1. Clique no seletor de período
2. Escolhe "Últimos 7 dias"
3. Todos os gráficos e KPIs atualizam

### 5.2. Comparar com Período Anterior
1. Check "Comparar com período anterior"
2. KPIs mostram tendência (+12%, -5%)
3. Gráficos mostram 2 linhas (atual vs anterior)

### 5.3. Exportar Relatório
1. Vai para tab "Exportar"
2. Seleciona relatório, formato, período
3. Clique "Exportar"
4. Download de arquivo (CSV/PDF/JSON)

### 5.4. Drill-down em Lesson
1. Clique em lesson no Top Lessons
2. Modal abre com detalhes da lesson:
   - Taxa de conclusão
   - Tempo médio de conclusão
   - Checkpoints com mais erros
   - Alunos que completaram vs desistiram

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Métricas Calculadas

**Alunos Ativos**: Logaram pelo menos 1x no período
**Taxa de Conclusão**: (Alunos que completaram lesson / Total de alunos que começaram) * 100
**Taxa de Retenção**: (Alunos que voltaram após 1ª sessão / Total de alunos) * 100
**Taxa de Conversão (Loja)**: (Alunos que compraram / Total de alunos) * 100

### 6.2. Agregação de Dados

- Dados agregados diariamente (não real-time)
- Cache de 1 hora para gráficos
- Recalcula KPIs a cada mudança de período

### 6.3. Privacidade

- Criador NÃO vê dados pessoais sensíveis em analytics (apenas agregados)
- Para ver dados individuais, usa tab "Usuários"

---

## 7. RESPONSIVO

**Desktop**: Gráficos lado a lado
**Tablet/Mobile**: Gráficos empilhados (verticalmente)

---

## 8. ANALYTICS (Meta-tracking)

**Eventos**:
- `admin_analytics_viewed`: Ao acessar Analytics (param: tab)
- `admin_analytics_period_changed`: Ao mudar período
- `admin_analytics_exported`: Ao exportar relatório (param: type, format)

---

## 9. ACESSIBILIDADE

- Gráficos têm tabela de dados acessível (hidden para sighted users)
- KPIs têm `aria-label` descritivo
- Navegação por teclado funciona

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/analytics/overview`: Visão geral
- `GET /api/admin/analytics/engagement`: Engajamento
- `GET /api/admin/analytics/lessons`: Lições
- `GET /api/admin/analytics/gamification`: Gamificação
- `GET /api/admin/analytics/store`: Loja
- `POST /api/admin/analytics/export`: Exportar relatório

**Query params**:
- `period`: `7d`, `30d`, `90d`, `all`, `custom`
- `start_date`: Data início (se period=custom)
- `end_date`: Data fim (se period=custom)
- `compare`: `true` (comparar com período anterior)

**Exemplo de resposta** (overview):
```json
{
  "kpis": {
    "total_learners": 245,
    "active_learners": 182,
    "completion_rate": 74.2,
    "retention_rate": 42.0
  },
  "trends": {
    "total_learners": 12.5,  // % vs período anterior
    "active_learners": 5.2,
    "completion_rate": -2.1,
    "retention_rate": 8.3
  },
  "charts": {
    "active_learners_daily": {
      "labels": ["Jan 1", "Jan 2", ...],
      "data": [45, 50, 42, ...]
    },
    "funnel": {
      "stages": ["Signups", "Onboarded", "1st Lesson", "5+ Lessons", "Completed"],
      "values": [300, 245, 200, 150, 100]
    }
  }
}
```

**Bibliotecas de Gráficos**:
- Chart.js (simples, leve)
- Recharts (React)
- D3.js (complexo, customizável)

**Recomendação**: Chart.js para v1 (simplicidade)

---

## 11. PERGUNTAS PARA O CLIENTE

1. **Analytics real-time**: Dados atualizados em tempo real ou agregados diariamente é suficiente?
2. **Exportação PDF**: Design do relatório PDF é importante v1 ou CSV é suficiente?
3. **Alertas**: Criador recebe alertas automáticos? (ex: "Taxa de conclusão caiu 20% esta semana")
4. **Comparação entre InfoApps**: Criador com múltiplos InfoApps pode comparar métricas entre eles? (v1 ou v1.1?)

---

**Status**: DRAFT
**Próxima revisão**: [Data]
