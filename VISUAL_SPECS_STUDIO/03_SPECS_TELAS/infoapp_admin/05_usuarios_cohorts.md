# InfoApp Admin - Usuários e Cohorts

**App**: InfoApp Admin Panel
**Tela**: Usuários/Cohorts (Gestão de Alunos)
**Versão**: 2.0
**Data**: 2025-12-26

---

## 1. CONTEXTO

**O que é**: Interface para criador gerenciar alunos do InfoApp. Visualizar lista de alunos, criar cohorts (turmas), gerenciar permissões.

**Quando**: Criador acessa via sidebar → "Usuários"

**Usuário**: Criador (dono do InfoApp)

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           👥 Usuários                                │
│                                                                │
│                     [Alunos] [Cohorts]  (tabs)                │
│                     ─────────────────────────────────────────  │
│                     [+ Convidar Aluno]  [📥 Exportar]         │
│                     🔍 Buscar alunos...                        │
│                     [Todos] [Ativos] [Inativos] [Cohort: ▼]   │
│                     ─────────────────────────────────────────  │
│                     📊 Resumo:                                 │
│                     245 alunos • 182 ativos (7d) • 74% concl.  │
│                     ─────────────────────────────────────────  │
│                     [Tabela de Alunos]                         │
│                     ┌────┬──────────┬──────┬────────┬────────┐│
│                     │ ☐  │ Nome     │ XP   │ Streak │ Ações  ││
│                     ├────┼──────────┼──────┼────────┼────────┤│
│                     │ ☐  │ João S.  │ 1245 │ 7 dias │ [...]  ││
│                     │ ☐  │ Maria O. │ 980  │ 3 dias │ [...]  ││
│                     │ ☐  │ Pedro A. │ 750  │ 0 dias │ [...]  ││
│                     │ ...│          │      │        │        ││
│                     └────┴──────────┴──────┴────────┴────────┘│
│                     [1-20 de 245]  [< 1 2 3 ... 13 >]         │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Tabs

**Opções**:
1. **Alunos** (tab padrão): Lista de todos os alunos
2. **Cohorts**: Gestão de turmas/grupos

### 3.2. Header (Tab Alunos)

**Elementos**:
- Botão: "+ Convidar Aluno" (envia convite por email)
- Botão: "📥 Exportar" (exporta CSV com lista de alunos)
- Search: Campo de busca (filtra por nome, email)

### 3.3. Filtros

**Filtros rápidos**:
- Todos (padrão)
- Ativos (logou nos últimos 7 dias)
- Inativos (não logou nos últimos 30 dias)
- Cohort: Dropdown (filtra por turma)

### 3.4. Resumo de Métricas

**KPIs**:
- Total de alunos: `245`
- Alunos ativos (7 dias): `182`
- Taxa de conclusão média: `74%`

### 3.5. Tabela de Alunos

**Colunas**:
1. **Checkbox**: Selecionar múltiplos alunos (ações em massa)
2. **Avatar + Nome**: Foto + nome do aluno
3. **Email**: email@example.com
4. **XP**: Total de XP
5. **Nível**: Nível atual
6. **Streak**: Dias consecutivos
7. **Coins**: Saldo de Coins
8. **Última atividade**: "2 horas atrás"
9. **Status**: Ativo (✅) ou Inativo (❌)
10. **Ações**: [...] (dropdown: Ver Perfil, Editar, Resetar Progresso, Remover)

**Ordenação**: Clique no header da coluna para ordenar (asc/desc)

**Paginação**: 20 alunos por página

### 3.6. Ações em Massa

**Quando alunos selecionados (checkbox)**:

Barra aparece:
```
┌────────────────────────────────────────┐
│ 3 alunos selecionados                  │
│ [Adicionar a Cohort]                   │
│ [Enviar Mensagem]                      │
│ [Exportar Selecionados]                │
│ [Remover]                              │
└────────────────────────────────────────┘
```

### 3.7. Dropdown de Ações (Individual)

**Clique [...] no aluno**:
- **Ver Perfil** → Abre modal com perfil detalhado
- **Editar** → Editar dados (nome, email, cohort)
- **Creditar Coins** → Adicionar/remover Coins manualmente
- **Creditar XP** → Adicionar/remover XP manualmente
- **Resetar Progresso** → Zera progresso (confirmação)
- **Remover** → Remove aluno do InfoApp (confirmação)

### 3.8. Modal: Ver Perfil do Aluno

```
┌────────────────────────────────────────────────────────────────┐
│ ✕  Perfil: João Silva                                          │
├────────────────────────────────────────────────────────────────┤
│  [Avatar]  João Silva                                          │
│            joao@example.com                                    │
│            Cohort: Turma Jan/2025                              │
│  ─────────────────────────────────────────────────────────────│
│  📊 Progresso:                                                 │
│     XP: 1.245  |  Nível: 7  |  Streak: 7 dias                 │
│     Coins: 245  |  Conclusão: 85% (17/20 lessons)             │
│  ─────────────────────────────────────────────────────────────│
│  📚 Lessons Completadas (17):                                  │
│     ✅ Intro Python (95% acertos)                              │
│     ✅ Variáveis (100% acertos)                                │
│     ✅ Funções (82% acertos)                                   │
│     ...                                                        │
│  ─────────────────────────────────────────────────────────────│
│  🏆 Badges (5):                                                │
│     🌱 Iniciante  🔥 Aprendiz  ⭐ Dedicado                     │
│  ─────────────────────────────────────────────────────────────│
│  🛒 Compras na Loja (3):                                       │
│     - Tema Escuro (50 coins)                                   │
│     - Freeze Streak (30 coins)                                 │
│     - Certificado (100 coins)                                  │
│  ─────────────────────────────────────────────────────────────│
│  📅 Atividade:                                                 │
│     Cadastro: 2024-12-01                                       │
│     Última atividade: 2 horas atrás                            │
│     Total de sessões: 45                                       │
│     Tempo médio/sessão: 18 min                                 │
│  ─────────────────────────────────────────────────────────────│
│                                            [Fechar]            │
└────────────────────────────────────────────────────────────────┘
```

### 3.9. Modal: Convidar Aluno

```
┌────────────────────────────────────────┐
│ ✕  Convidar Aluno                      │
├────────────────────────────────────────┤
│  Email(s):                             │
│  [joao@example.com, maria@...]         │
│  (separe múltiplos emails por vírgula) │
│                                        │
│  Cohort (opcional):                    │
│  [Dropdown: Turma Jan/2025 ▼]         │
│                                        │
│  Mensagem personalizada (opcional):    │
│  [Campo de texto..................]   │
│                                        │
│  ─────────────────────────────────────│
│           [Cancelar]  [Enviar Convite]│
└────────────────────────────────────────┘
```

**Após enviar**:
- Toast: "Convites enviados para 2 alunos! 📧"
- Alunos recebem email com link de cadastro

### 3.10. Tab: Cohorts

**Lista de Cohorts (Turmas)**

```
┌────────────────────────────────────────────────────────────────┐
│ [+ Nova Cohort]                                                │
│ ─────────────────────────────────────────────────────────────  │
│ 📂 Turma Jan/2025 (45 alunos)                                 │
│    Criado em: 2025-01-01                                       │
│    Conclusão média: 78%                                        │
│    [Ver Alunos] [Editar] [Deletar]                            │
│ ─────────────────────────────────────────────────────────────  │
│ 📂 Turma Fev/2025 (32 alunos)                                 │
│    Criado em: 2025-02-01                                       │
│    Conclusão média: 65%                                        │
│    [Ver Alunos] [Editar] [Deletar]                            │
│ ─────────────────────────────────────────────────────────────  │
│ ...                                                            │
└────────────────────────────────────────────────────────────────┘
```

**Ações**:
- **Ver Alunos** → Redireciona para tab Alunos (filtrado por cohort)
- **Editar** → Modal para editar nome/descrição
- **Deletar** → Confirmação (alunos não são deletados, apenas removidos da cohort)

### 3.11. Modal: Nova Cohort

```
┌────────────────────────────────────────┐
│ ✕  Nova Cohort                         │
├────────────────────────────────────────┤
│  Nome: [Turma Jan/2025...........]     │
│  Descrição: [Campo de texto...]       │
│                                        │
│  Adicionar alunos:                     │
│  [Multi-select: João, Maria, Pedro...] │
│  (ou deixe vazio e adicione depois)    │
│                                        │
│  ─────────────────────────────────────│
│              [Cancelar]  [Criar]      │
└────────────────────────────────────────┘
```

---

## 4. ESTADOS

### 4.1. Empty State (sem alunos)
```
┌────────────────────────────────────────┐
│  👥 Nenhum aluno cadastrado            │
│  Convide seus primeiros alunos!        │
│                                        │
│  [+ Convidar Alunos]                  │
│  [Compartilhar Link de Inscrição]     │
└────────────────────────────────────────┘
```

### 4.2. Loading
- Skeleton para tabela de alunos

### 4.3. Erro
- Toast: "Erro ao carregar alunos. Tente novamente."

### 4.4. Sucesso
- Toast: "Convites enviados! 📧"
- Toast: "Aluno removido."
- Toast: "Coins creditados com sucesso!"

---

## 5. INTERAÇÕES

### 5.1. Convidar Aluno
1. Clique "+ Convidar Aluno"
2. Modal abre
3. Digita email(s)
4. (Opcional) Seleciona cohort
5. (Opcional) Escreve mensagem personalizada
6. Clique "Enviar Convite"
7. Email de convite é enviado com link de cadastro

### 5.2. Ver Perfil de Aluno
1. Clique [...] no aluno → "Ver Perfil"
2. Modal abre com dados completos
3. Criador vê progresso, badges, compras, atividade

### 5.3. Creditar Coins/XP Manualmente
1. Clique [...] no aluno → "Creditar Coins"
2. Modal:
   ```
   Creditar Coins para João Silva
   Quantidade: [+100] coins
   Motivo (opcional): [Bônus especial...]
   [Cancelar]  [Creditar]
   ```
3. Confirma → Coins são adicionados
4. Aluno recebe notificação: "Você ganhou 100 Coins! 🎉"

### 5.4. Resetar Progresso de Aluno
1. Clique [...] no aluno → "Resetar Progresso"
2. Modal:
   ```
   ⚠️  Resetar progresso de João Silva?
   Isso irá:
   - Zerar XP e Nível
   - Zerar Coins
   - Marcar todas as lessons como não completadas
   - Manter histórico de compras (não reembolsa)

   Esta ação não pode ser desfeita.
   [Cancelar]  [Resetar]
   ```
3. Confirma → Progresso é zerado

### 5.5. Remover Aluno
1. Clique [...] no aluno → "Remover"
2. Modal:
   ```
   ⚠️  Remover João Silva do InfoApp?
   Aluno perderá acesso ao InfoApp.
   Dados serão mantidos por 30 dias (pode restaurar).
   [Cancelar]  [Remover]
   ```
3. Confirma → Aluno é removido (soft delete)

### 5.6. Ações em Massa
1. Seleciona múltiplos alunos (checkbox)
2. Barra de ações aparece
3. Clique "Adicionar a Cohort"
4. Seleciona cohort
5. Confirma → Alunos são adicionados à cohort

### 5.7. Criar Cohort
1. Vai para tab "Cohorts"
2. Clique "+ Nova Cohort"
3. Modal abre
4. Preenche nome, descrição, seleciona alunos
5. Clique "Criar" → Cohort criada

### 5.8. Exportar Lista de Alunos
1. Clique "📥 Exportar"
2. Modal:
   ```
   Exportar Alunos
   Formato: [CSV ▼]
   Incluir colunas:
   ☑️  Nome, Email, XP, Nível, Streak, Coins
   ☑️  Última atividade
   ☑️  Cohort
   ☐  Histórico de compras
   [Cancelar]  [Exportar]
   ```
3. Clique "Exportar" → Download de arquivo CSV

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Convite por Email
- Email enviado com link único: `{infoapp-slug}.plataforma.com/signup?invite={token}`
- Token expira em 7 dias
- Ao clicar, aluno é direcionado para signup com InfoApp pré-selecionado

### 6.2. Cohorts (Turmas)
- Aluno pode estar em múltiplas cohorts
- Cohorts são organizacionais (não afetam funcionalidade do app)
- Futuro (v1.1): Cohorts podem ter conteúdo exclusivo

### 6.3. Creditar Coins/XP Manualmente
- Criador pode adicionar ou remover (valores negativos)
- Histórico de transações manuais é registrado (auditoria)

### 6.4. Remover Aluno
- Soft delete: Dados mantidos por 30 dias (LGPD)
- Aluno perde acesso ao InfoApp imediatamente
- Criador pode restaurar aluno dentro de 30 dias

### 6.5. Resetar Progresso
- XP, Coins, conclusão de lessons são zerados
- Histórico de compras é mantido (não reembolsa)
- Badges são removidos

---

## 7. RESPONSIVO

**Desktop**: Tabela completa
**Tablet**: Tabela com scroll horizontal
**Mobile**: Cards (lista vertical) ao invés de tabela

---

## 8. ANALYTICS (Tracking)

**Eventos**:
- `admin_users_viewed`: Ao acessar tab Alunos
- `admin_user_invited`: Ao enviar convite (param: count)
- `admin_user_profile_viewed`: Ao ver perfil de aluno
- `admin_user_coins_credited`: Ao creditar Coins (param: amount)
- `admin_user_removed`: Ao remover aluno
- `admin_cohort_created`: Ao criar cohort
- `admin_users_exported`: Ao exportar lista

---

## 9. ACESSIBILIDADE

- Tabela navegável por teclado
- Screen reader anuncia quantidade de alunos
- Modais têm foco automático no primeiro campo

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/users`: Lista alunos (com filtros, paginação)
- `GET /api/admin/users/:id`: Perfil detalhado de aluno
- `POST /api/admin/users/invite`: Envia convite por email
- `PUT /api/admin/users/:id`: Edita aluno
- `DELETE /api/admin/users/:id`: Remove aluno (soft delete)
- `POST /api/admin/users/:id/credit-coins`: Credita Coins
- `POST /api/admin/users/:id/credit-xp`: Credita XP
- `POST /api/admin/users/:id/reset-progress`: Reseta progresso
- `GET /api/admin/cohorts`: Lista cohorts
- `POST /api/admin/cohorts`: Cria cohort
- `PUT /api/admin/cohorts/:id`: Atualiza cohort
- `DELETE /api/admin/cohorts/:id`: Deleta cohort
- `POST /api/admin/users/export`: Exporta CSV

**Exemplo de resposta** (lista de alunos):
```json
{
  "data": [
    {
      "id": "user_123",
      "name": "João Silva",
      "email": "joao@example.com",
      "xp_total": 1245,
      "level": 7,
      "streak_days": 7,
      "coins_balance": 245,
      "last_activity": "2025-01-15T14:30:00Z",
      "completion_rate": 85,
      "cohorts": ["cohort_jan2025"],
      "status": "active"
    },
    ...
  ],
  "meta": {
    "total": 245,
    "page": 1,
    "per_page": 20
  }
}
```

---

**Status**: DRAFT
**Próxima revisão**: [Data]
