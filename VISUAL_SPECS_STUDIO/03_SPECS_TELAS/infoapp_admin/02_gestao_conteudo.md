# InfoApp Admin - Gestão de Conteúdo

**App**: InfoApp Admin Panel
**Tela**: Gestão de Conteúdo (CRUD de Lessons/Beats/Checkpoints)
**Versão**: 2.0
**Data**: 2025-12-26
**Mudança**: Novo componente (gestão detalhada de conteúdo movida para InfoApp Admin)

---

## 1. CONTEXTO

**O que é**: Interface para criador criar, editar, visualizar e deletar Lessons, Beats e Checkpoints do InfoApp.

**Quando**: Criador acessa via sidebar → "Gestão de Conteúdo"

**Usuário**: Criador (dono do InfoApp)

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           📚 Gestão de Conteúdo                      │
│                                                                │
│                     [+ Nova Lesson]  [Importar]  [🔍 Buscar]  │
│                     ─────────────────────────────────────────  │
│                     📂 Estação: Fundamentos (12 lessons)       │
│                     ┌──────────────────────────────────────┐  │
│                     │ Lesson 1: Introdução ao Python       │  │
│                     │ 5 beats • 3 checkpoints • 95% concl. │  │
│                     │ [Editar] [Preview] [Duplicar] [🗑️]   │  │
│                     └──────────────────────────────────────┘  │
│                     ┌──────────────────────────────────────┐  │
│                     │ Lesson 2: Variáveis e Tipos          │  │
│                     │ 4 beats • 2 checkpoints • 89% concl. │  │
│                     │ [Editar] [Preview] [Duplicar] [🗑️]   │  │
│                     └──────────────────────────────────────┘  │
│                     ...                                        │
│                     ─────────────────────────────────────────  │
│                     📂 Estação: Intermediário (8 lessons)      │
│                     ...                                        │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Header da Tela

**Elementos**:
- Título: "Gestão de Conteúdo"
- Botão: "+ Nova Lesson" (primário)
- Botão: "Importar" (secundário, redireciona para Upload em Massa)
- Search: Campo de busca (filtra lessons por nome)

### 3.2. Agrupamento por Estação

**Visual**:
```
📂 Estação: Fundamentos (12 lessons)
   [Collapse/Expand]
```

**Estações**: Configuradas ao criar InfoApp no Modo Studio

**Ações**:
- Clique em título → Collapse/expand lessons da estação
- Reordenar estações (drag-and-drop)

### 3.3. Card de Lesson

**Informações**:
- Título da lesson
- Resumo: "5 beats • 3 checkpoints • 95% conclusão"
- Status: Draft / Publicado (badge)

**Ações**:
1. **Editar** → Abre editor de lesson (modal ou página)
2. **Preview** → Preview da lesson no emulador (player)
3. **Duplicar** → Cria cópia da lesson
4. **Deletar** (🗑️) → Confirmação + deleta lesson

**Interação**: Hover mostra ações

### 3.4. Editor de Lesson (Modal)

**Aberto ao clicar "Editar" ou "+ Nova Lesson"**

```
┌────────────────────────────────────────────────────────────────┐
│ ✕  Editar Lesson: Introdução ao Python                        │
├────────────────────────────────────────────────────────────────┤
│  [Básico] [Beats] [Checkpoints] [Configurações]               │
│                                                                │
│  BÁSICO:                                                       │
│  Título: [Introdução ao Python................]               │
│  Descrição: [Campo de texto longo............]                │
│  Estação: [Dropdown: Fundamentos ▼]                           │
│  XP Reward: [50........]                                       │
│  Coins Reward: [10........]                                    │
│                                                                │
│  ─────────────────────────────────────────────────────────────│
│                               [Cancelar]  [Salvar Rascunho]   │
│                                           [Publicar]           │
└────────────────────────────────────────────────────────────────┘
```

**Tabs**:
1. **Básico**: Título, descrição, estação, rewards
2. **Beats**: Editor de beats (lista + CRUD)
3. **Checkpoints**: Editor de checkpoints (lista + CRUD)
4. **Configurações**: Avançado (pré-requisitos, unlock conditions)

### 3.5. Editor de Beats (Tab)

```
┌────────────────────────────────────────────────────────────────┐
│  BEATS (5 beats)                              [+ Adicionar]    │
│  ─────────────────────────────────────────────────────────────│
│  1. [Text] "Python é uma linguagem..."                        │
│     Áudio: [🔊 Ouvir]  [🔄 Regerar TTS]                       │
│     [⬆️] [⬇️] [✏️] [🗑️]                                        │
│  ─────────────────────────────────────────────────────────────│
│  2. [Image] "Exemplo de código"                               │
│     Imagem: [📷 Ver]  [🔄 Trocar]                             │
│     [⬆️] [⬇️] [✏️] [🗑️]                                        │
│  ─────────────────────────────────────────────────────────────│
│  ...                                                           │
└────────────────────────────────────────────────────────────────┘
```

**Tipos de Beat**:
- Text (com TTS)
- Image (com caption opcional)
- Video (embed ou upload)
- Code (syntax highlighting)

**Ações**:
- **Adicionar**: Modal para escolher tipo de beat + criar
- **Reordenar**: Drag-and-drop ou setas ⬆️⬇️
- **Editar**: Modal para editar conteúdo do beat
- **Deletar**: Confirmação + deleta

**TTS**:
- Ao criar beat de texto, TTS é gerado automaticamente
- Botão "Regerar TTS" se criador editar texto
- Idioma: Baseado no idioma principal do InfoApp

### 3.6. Editor de Checkpoints (Tab)

```
┌────────────────────────────────────────────────────────────────┐
│  CHECKPOINTS (3 checkpoints)                  [+ Adicionar]    │
│  ─────────────────────────────────────────────────────────────│
│  1. [Match] Associe comandos Python                           │
│     Posição: Após beat 2                                      │
│     Pares: 3 (print → exibir texto, input → ler dados, ...)   │
│     XP: 10  Coins: 2                                          │
│     [✏️] [🗑️]                                                  │
│  ─────────────────────────────────────────────────────────────│
│  2. [MultipleChoice] Qual é a saída de print(2+2)?            │
│     Posição: Após beat 4                                      │
│     Opções: 4, 22, "4", erro (correto: 4)                     │
│     XP: 10  Coins: 2                                          │
│     [✏️] [🗑️]                                                  │
│  ─────────────────────────────────────────────────────────────│
│  ...                                                           │
└────────────────────────────────────────────────────────────────┘
```

**Tipos de Checkpoint**:
- Multiple Choice
- Match (arrastar pares)
- Fill in the Blank
- Code (executar código e validar output)

**Ações**:
- **Adicionar**: Modal para escolher tipo + criar checkpoint
- **Editar**: Modal para editar pergunta/opções
- **Deletar**: Confirmação + deleta

---

## 4. ESTADOS

### 4.1. Empty State (sem lessons)
```
┌────────────────────────────────────────┐
│  📚 Nenhuma lesson criada ainda        │
│  Comece criando sua primeira lesson!   │
│                                        │
│  [+ Criar Lesson]  [Importar CSV]     │
└────────────────────────────────────────┘
```

### 4.2. Loading
- Skeleton cards para lessons
- Spinner ao salvar lesson

### 4.3. Erro
- Toast: "Erro ao salvar lesson. Tente novamente."
- Validação: "Título é obrigatório" (campo em vermelho)

### 4.4. Sucesso
- Toast: "Lesson criada com sucesso! 🎉"
- Toast: "Lesson publicada!"

---

## 5. INTERAÇÕES

### 5.1. Criar Nova Lesson
1. Clique "+ Nova Lesson"
2. Modal abre (tab "Básico")
3. Preenche título, descrição, estação
4. Vai para tab "Beats" → Adiciona beats
5. Vai para tab "Checkpoints" → Adiciona checkpoints
6. Clique "Salvar Rascunho" (salva sem publicar) ou "Publicar" (salva e publica)
7. Modal fecha, lesson aparece na lista

### 5.2. Editar Lesson Existente
1. Clique "Editar" no card da lesson
2. Modal abre com dados preenchidos
3. Criador edita (beats, checkpoints, etc.)
4. Clique "Salvar" → Atualiza lesson
5. Se lesson já estava publicada, mudanças são refletidas imediatamente (ou criar sistema de versionamento)

### 5.3. Preview de Lesson
1. Clique "Preview" no card da lesson
2. Abre emulador/player (idêntico ao Learner App)
3. Criador vê lesson como aluno veria
4. Pode testar checkpoints

### 5.4. Duplicar Lesson
1. Clique "Duplicar"
2. Cria cópia da lesson com nome "Cópia de [título original]"
3. Cópia fica em estado "Draft"

### 5.5. Deletar Lesson
1. Clique 🗑️
2. Modal: "Tem certeza? Esta ação não pode ser desfeita."
3. Confirma → Lesson é deletada
4. Se alunos já começaram lesson, avisar: "15 alunos já começaram esta lesson. Deletar mesmo assim?"

### 5.6. Buscar Lesson
1. Digite no campo de busca
2. Filtra lista de lessons (busca por título, descrição)
3. Resultados destacam termo buscado

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Publicação
- **Draft**: Lesson invisível para alunos
- **Publicado**: Lesson visível e acessível para alunos

### 6.2. Edição de Lesson Publicada
**Opção A** (v1 - simples): Edições refletem imediatamente
**Opção B** (v1.1 - robusto): Versionamento (edição cria nova versão, alunos que já começaram continuam na versão antiga)

**Decisão**: Opção A para v1

### 6.3. Ordem de Beats/Checkpoints
- Beats são exibidos na ordem definida (sequencial)
- Checkpoints aparecem após o beat especificado ("Após beat 2")

### 6.4. TTS Automático
- Ao criar beat de texto, TTS é gerado automaticamente via ElevenLabs
- Idioma: Baseado no idioma principal do InfoApp (configurado no Modo Studio)
- Se criador editar texto do beat, precisa clicar "Regerar TTS" (não automático, para economizar API calls)

### 6.5. Validações
- Título da lesson: obrigatório, max 100 caracteres
- Descrição: opcional, max 500 caracteres
- Lesson precisa ter no mínimo 1 beat
- Checkpoint precisa ter opção correta marcada

---

## 7. RESPONSIVO

**Desktop**: Editor em modal fullscreen
**Tablet/Mobile**: Editor em página inteira (não modal)

---

## 8. ANALYTICS (Tracking)

**Eventos**:
- `admin_lesson_created`: Ao criar lesson
- `admin_lesson_edited`: Ao editar lesson
- `admin_lesson_published`: Ao publicar lesson
- `admin_lesson_deleted`: Ao deletar lesson
- `admin_beat_created`: Ao criar beat (param: type)
- `admin_checkpoint_created`: Ao criar checkpoint (param: type)
- `admin_tts_regenerated`: Ao regerar TTS

---

## 9. ACESSIBILIDADE

- Editor de lesson navegável por teclado
- Screen reader anuncia "Editar lesson: [título]" ao abrir modal
- Botões de ação têm `aria-label` descritivo

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/lessons`: Lista lessons (com filtros)
- `POST /api/admin/lessons`: Cria lesson
- `PUT /api/admin/lessons/:id`: Atualiza lesson
- `DELETE /api/admin/lessons/:id`: Deleta lesson
- `POST /api/admin/lessons/:id/publish`: Publica lesson
- `POST /api/admin/beats`: Cria beat
- `PUT /api/admin/beats/:id`: Atualiza beat
- `DELETE /api/admin/beats/:id`: Deleta beat
- `POST /api/admin/checkpoints`: Cria checkpoint
- `PUT /api/admin/checkpoints/:id`: Atualiza checkpoint
- `DELETE /api/admin/checkpoints/:id`: Deleta checkpoint
- `POST /api/admin/tts/generate`: Gera áudio TTS para beat

**Exemplo de payload** (criar lesson):
```json
{
  "title": "Introdução ao Python",
  "description": "Aprenda os fundamentos da linguagem Python",
  "station_id": "station_fundamentos",
  "xp_reward": 50,
  "coins_reward": 10,
  "status": "draft"
}
```

**Exemplo de payload** (criar beat):
```json
{
  "lesson_id": "lesson_123",
  "type": "text",
  "order": 1,
  "content": {
    "text": "Python é uma linguagem de programação de alto nível.",
    "audio_url": null  // será gerado via TTS
  }
}
```

---

**Status**: DRAFT
**Próxima revisão**: [Data]
