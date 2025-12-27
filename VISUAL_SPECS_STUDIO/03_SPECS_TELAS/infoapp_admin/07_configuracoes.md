# InfoApp Admin - Configurações

**App**: InfoApp Admin Panel
**Tela**: Configurações (Settings)
**Versão**: 2.0
**Data**: 2025-12-26

---

## 1. CONTEXTO

**O que é**: Interface para criador configurar o InfoApp. Branding, idioma, domínio, integrações, preferências avançadas.

**Quando**: Criador acessa via sidebar → "Configurações"

**Usuário**: Criador (dono do InfoApp)

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           ⚙️  Configurações                          │
│                                                                │
│                     [Geral] [Branding] [Domínio] [Integrações] │
│                     [Avançado] [Perigos]                       │
│                     ─────────────────────────────────────────  │
│                     📝 Informações Gerais                      │
│                                                                │
│                     Nome do InfoApp:                           │
│                     [Aprenda Python................]           │
│                                                                │
│                     Descrição:                                 │
│                     [Campo de texto longo............]         │
│                                                                │
│                     Idioma Principal:                          │
│                     [Dropdown: Português (Brasil) ▼]          │
│                                                                │
│                     Visibilidade:                              │
│                     (●) Público (aceita novos signups)         │
│                     ( ) Privado (apenas por convite)           │
│                                                                │
│                     [Salvar Alterações]                        │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Tabs de Categorias

**Tabs**:
1. **Geral** (padrão): Nome, descrição, idioma, visibilidade
2. **Branding**: Logo, cores, tema visual
3. **Domínio**: Subdomínio, domínio customizado
4. **Integrações**: Webhooks, API, OAuth
5. **Avançado**: Gamificação, notificações, preferências
6. **Perigos**: Resetar InfoApp, deletar InfoApp

---

### 3.2. Tab: Geral

```
┌────────────────────────────────────────┐
│ 📝 Informações Gerais                  │
│ ─────────────────────────────────────  │
│ Nome do InfoApp:                       │
│ [Aprenda Python................]       │
│                                        │
│ Descrição (aparece no signup):         │
│ [Curso interativo de Python...]       │
│                                        │
│ Idioma Principal do Conteúdo:          │
│ [Dropdown: Português (Brasil) ▼]      │
│ (áudios TTS serão gerados neste idioma)│
│                                        │
│ Visibilidade:                          │
│ (●) Público                            │
│     Qualquer pessoa pode se cadastrar  │
│ ( ) Privado                            │
│     Apenas convidados podem acessar    │
│                                        │
│ [Salvar Alterações]                    │
└────────────────────────────────────────┘
```

**Campos**:
- Nome do InfoApp (max 100 caracteres)
- Descrição (max 500 caracteres)
- Idioma principal (PT-BR, EN-US, ES-ES)
- Visibilidade (público ou privado)

---

### 3.3. Tab: Branding

```
┌────────────────────────────────────────┐
│ 🎨 Branding                            │
│ ─────────────────────────────────────  │
│ Logo:                                  │
│ [Preview do logo]                      │
│ [📷 Upload Logo]  [Remover]           │
│ Formato: PNG, SVG (max 2 MB)           │
│                                        │
│ Cores:                                 │
│ Cor Primária: [#3B82F6] [Color picker]│
│ Cor Secundária: [#10B981] [...]       │
│                                        │
│ Tema Padrão:                           │
│ (●) Claro                              │
│ ( ) Escuro                             │
│ ( ) Automático (segue sistema do aluno)│
│                                        │
│ Favicon:                               │
│ [Preview]                              │
│ [📷 Upload]  [Usar Logo]              │
│                                        │
│ [Preview do InfoApp]                   │
│ (mostra como ficará com branding)      │
│                                        │
│ [Salvar Alterações]                    │
└────────────────────────────────────────┘
```

**Campos**:
- Logo (upload de imagem)
- Cor primária (color picker)
- Cor secundária
- Tema padrão (claro/escuro/automático)
- Favicon (ícone do navegador)

**Preview**: Botão para abrir emulador com branding aplicado

---

### 3.4. Tab: Domínio

```
┌────────────────────────────────────────┐
│ 🌐 Domínio                             │
│ ─────────────────────────────────────  │
│ Subdomínio Atual:                      │
│ aprenda-python.plataforma.com          │
│                                        │
│ Mudar Subdomínio:                      │
│ [novo-slug].plataforma.com             │
│ ⚠️  Cuidado: Links antigos quebrarão   │
│ [Mudar Subdomínio]                     │
│                                        │
│ ─────────────────────────────────────  │
│ Domínio Customizado (v1.1):            │
│ [Configurar domínio próprio]           │
│ (ex: meuapp.com)                       │
│                                        │
│ Status: Não configurado                │
│ [Configurar Domínio]                   │
└────────────────────────────────────────┘
```

**Funcionalidades**:
- Visualizar subdomínio atual
- Mudar subdomínio (com confirmação)
- Configurar domínio customizado (v1.1)

**Validações**:
- Subdomínio: Apenas letras, números, hífen (a-z, 0-9, -)
- Sem espaços, caracteres especiais
- Verificar disponibilidade (não pode estar em uso por outro InfoApp)

---

### 3.5. Tab: Integrações

```
┌────────────────────────────────────────┐
│ 🔌 Integrações                         │
│ ─────────────────────────────────────  │
│ API Keys:                              │
│ [Gerar Nova API Key]                   │
│ - key_abc123... (criada em 2025-01-01) │
│   [Copiar] [Revogar]                   │
│                                        │
│ ─────────────────────────────────────  │
│ Webhooks:                              │
│ [+ Adicionar Webhook]                  │
│ - https://api.exemplo.com/webhook      │
│   Eventos: lesson_completed, signup    │
│   [Editar] [Deletar] [Testar]         │
│                                        │
│ ─────────────────────────────────────  │
│ OAuth / SSO (v1.1):                    │
│ ☐ Google Login                         │
│ ☐ Microsoft Login                      │
│ ☐ SAML SSO                             │
└────────────────────────────────────────┘
```

**Funcionalidades**:
- Gerar API Keys (para integrar com sistemas externos)
- Configurar Webhooks (enviar eventos para URLs externas)
- OAuth/SSO (v1.1)

**Webhooks - Eventos disponíveis**:
- `learner.signup`: Novo aluno se cadastrou
- `learner.lesson_completed`: Aluno completou lesson
- `learner.badge_earned`: Aluno ganhou badge
- `store.purchase`: Aluno comprou produto na loja

---

### 3.6. Tab: Avançado

```
┌────────────────────────────────────────┐
│ ⚙️  Configurações Avançadas            │
│ ─────────────────────────────────────  │
│ Gamificação:                           │
│ ☑️  Ativar sistema de XP e Coins       │
│ ☑️  Ativar Streaks                     │
│ ☑️  Ativar Ligas/Ranking               │
│ ☐  Modo competitivo (ranking público)  │
│                                        │
│ Notificações:                          │
│ ☑️  Emails de boas-vindas              │
│ ☑️  Emails de lembrete de streak       │
│ ☑️  Push notifications                 │
│ ☐  Notificações SMS (v1.1)             │
│                                        │
│ Preferências:                          │
│ Daily Goal Padrão: [50] XP/dia         │
│ Timezone: [America/Sao_Paulo ▼]       │
│                                        │
│ [Salvar Alterações]                    │
└────────────────────────────────────────┘
```

**Campos**:
- Toggles para ativar/desativar funcionalidades
- Daily goal padrão para novos alunos
- Timezone do InfoApp

---

### 3.7. Tab: Perigos

```
┌────────────────────────────────────────┐
│ ⚠️  Zona de Perigo                     │
│ ─────────────────────────────────────  │
│ Resetar Progresso de Todos os Alunos:  │
│ [Resetar Progresso]                    │
│ ⚠️  Zera XP, Coins, conclusão de todos │
│                                        │
│ ─────────────────────────────────────  │
│ Exportar Dados (LGPD):                 │
│ [Exportar Todos os Dados]              │
│ Baixa JSON com todos os dados do InfoApp│
│                                        │
│ ─────────────────────────────────────  │
│ Deletar InfoApp:                       │
│ [Deletar InfoApp]                      │
│ ⚠️  ATENÇÃO: Esta ação é IRREVERSÍVEL  │
│ Todos os dados serão perdidos.         │
│ Alunos perderão acesso.                │
└────────────────────────────────────────┘
```

**Ações perigosas**:
1. **Resetar Progresso**: Zera progresso de todos os alunos
2. **Exportar Dados**: Download de todos os dados (compliance LGPD)
3. **Deletar InfoApp**: Remove InfoApp permanentemente

**Confirmações**:
- Todas as ações perigosas exigem confirmação dupla
- Ex: "Digite o nome do InfoApp para confirmar"

---

## 4. ESTADOS

### 4.1. Loading
- Spinner ao salvar configurações

### 4.2. Erro
- Toast: "Erro ao salvar. Tente novamente."
- Validação: "Subdomínio já está em uso."

### 4.3. Sucesso
- Toast: "Configurações salvas! ✅"
- Toast: "Branding atualizado! Recarregue a página para ver."

---

## 5. INTERAÇÕES

### 5.1. Alterar Branding
1. Vai para tab "Branding"
2. Upload logo
3. Escolhe cores (color picker)
4. Clique "Preview" → Vê emulador com branding aplicado
5. Clique "Salvar" → Branding atualizado

### 5.2. Mudar Subdomínio
1. Vai para tab "Domínio"
2. Digita novo slug: "python-facil"
3. Valida disponibilidade
4. Modal de confirmação:
   ```
   ⚠️  Mudar subdomínio?
   De: aprenda-python.plataforma.com
   Para: python-facil.plataforma.com

   Links antigos quebrarão.
   Configure redirecionamento se necessário.

   Digite "python-facil" para confirmar:
   [...........]

   [Cancelar]  [Confirmar]
   ```
5. Confirma → Subdomínio atualizado

### 5.3. Gerar API Key
1. Vai para tab "Integrações"
2. Clique "Gerar Nova API Key"
3. Modal:
   ```
   Nova API Key gerada!
   key_abc123def456...

   ⚠️  Guarde em local seguro.
   Esta chave não será mostrada novamente.

   [Copiar]  [Fechar]
   ```
4. Copia chave → Usa em integração externa

### 5.4. Configurar Webhook
1. Clique "+ Adicionar Webhook"
2. Modal:
   ```
   Adicionar Webhook
   URL: [https://api.exemplo.com/webhook]
   Eventos:
   ☑️  learner.signup
   ☑️  learner.lesson_completed
   ☐  learner.badge_earned
   ☐  store.purchase

   [Cancelar]  [Adicionar]
   ```
3. Salva → Webhook configurado
4. Clique "Testar" → Envia payload de teste para URL

### 5.5. Deletar InfoApp
1. Vai para tab "Perigos"
2. Clique "Deletar InfoApp"
3. Modal:
   ```
   ⚠️  DELETAR INFOAPP?

   Esta ação é IRREVERSÍVEL.
   - Todos os dados serão perdidos
   - Alunos perderão acesso
   - Conteúdo será deletado

   Digite o nome do InfoApp para confirmar:
   [Aprenda Python]

   [Cancelar]  [Deletar Permanentemente]
   ```
4. Digita nome → Confirma → InfoApp deletado
5. Redireciona para Modo Studio

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Mudança de Subdomínio
- Criador pode mudar 1x a cada 30 dias (evitar spam)
- Subdomínio antigo fica indisponível por 7 dias (evitar phishing)

### 6.2. API Keys
- Máximo 5 API keys ativas por InfoApp
- API key não expira (mas pode ser revogada)

### 6.3. Webhooks
- Máximo 10 webhooks por InfoApp
- Timeout de 5 segundos (se URL não responder, marca como falho)
- Retry automático (3 tentativas com backoff)

### 6.4. Branding
- Logo max 2 MB (PNG, SVG)
- Cores em hex (#RRGGBB)

### 6.5. Deletar InfoApp
- Confirmação dupla obrigatória
- Soft delete por 30 dias (pode restaurar)
- Após 30 dias, hard delete (irrecuperável)

---

## 7. RESPONSIVO

**Desktop**: Tabs lado a lado
**Tablet/Mobile**: Tabs em dropdown, formulários verticais

---

## 8. ANALYTICS (Tracking)

**Eventos**:
- `admin_settings_viewed`: Ao acessar Configurações (param: tab)
- `admin_settings_saved`: Ao salvar (param: tab)
- `admin_branding_updated`: Ao atualizar branding
- `admin_subdomain_changed`: Ao mudar subdomínio
- `admin_api_key_generated`: Ao gerar API key
- `admin_webhook_created`: Ao criar webhook
- `admin_infoapp_deleted`: Ao deletar InfoApp

---

## 9. ACESSIBILIDADE

- Formulários navegáveis por teclado
- Color picker acessível
- Confirmações anunciadas por screen reader

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/settings`: Lista configurações
- `PUT /api/admin/settings/general`: Atualiza geral
- `PUT /api/admin/settings/branding`: Atualiza branding
- `POST /api/admin/settings/subdomain`: Muda subdomínio
- `POST /api/admin/settings/api-keys`: Gera API key
- `DELETE /api/admin/settings/api-keys/:id`: Revoga API key
- `POST /api/admin/settings/webhooks`: Cria webhook
- `PUT /api/admin/settings/webhooks/:id`: Atualiza webhook
- `DELETE /api/admin/settings/webhooks/:id`: Deleta webhook
- `DELETE /api/admin/settings/infoapp`: Deleta InfoApp

**Exemplo de payload** (atualizar branding):
```json
{
  "logo_url": "https://cdn.../logo.png",
  "primary_color": "#3B82F6",
  "secondary_color": "#10B981",
  "default_theme": "light",
  "favicon_url": "https://cdn.../favicon.png"
}
```

**Webhook Payload** (exemplo):
```json
{
  "event": "learner.lesson_completed",
  "infoapp_id": "infoapp_abc123",
  "timestamp": "2025-01-15T14:30:00Z",
  "data": {
    "learner_id": "user_123",
    "lesson_id": "lesson_001",
    "xp_earned": 50,
    "coins_earned": 10,
    "completion_rate": 100
  }
}
```

---

## 11. PERGUNTAS PARA O CLIENTE

1. **Domínio customizado**: Prioridade v1 ou v1.1?
2. **OAuth/SSO**: Quais provedores são prioritários? (Google, Microsoft, SAML?)
3. **White-label completo**: Criador pode remover marca "Powered by [Plataforma]"? (plano pago?)
4. **API rate limits**: Quantas chamadas/hora por API key?
5. **Webhooks retry**: 3 tentativas suficiente ou configurável?

---

**Status**: DRAFT
**Próxima revisão**: [Data]
