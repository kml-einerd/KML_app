# CATÁLOGO DE COMPONENTES

**Inventário completo de todos os componentes citados nas specs de telas**

Este catálogo lista TODOS os componentes reutilizáveis do produto com props, variantes e estados.

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Design Systems Lead → Componentes travados antes de pixel polish]

---

## PRINCÍPIOS DO DESIGN SYSTEM

1. **Finito**: Catálogo fechado (não inventar componentes novos)
2. **Reutilizável**: Mesmo componente em múltiplas telas
3. **Estados obrigatórios**: Loading, Empty, Error, Success
4. **Variantes por Tension**: SAFE/TENSION/STATUS

[fonte: 07 - alinhamento.md → Design Systems Lead → Componentes obrigatórios]

---

## COMPONENTES DO LEARNER APP

### ProgressHeader
**Onde usa**: Home, Progresso
**Props**:
- `avatar` (string URL)
- `nome` (string)
- `streak_dias` (number)
- `xp_atual` (number)
- `xp_proximo_nivel` (number)
- `nivel` (number)

**Variantes**:
- `streak_em_risco` (boolean) → exibe alerta laranja

**Comportamento**:
- Tap no streak abre modal explicando sistema
- Barra de progresso XP animada

**Estados**:
- Loading: Skeleton
- Success: Exibe dados completos

---

### MissionCard
**Onde usa**: Home
**Props**:
- `titulo` (string)
- `descricao` (string, opcional)
- `xp` (number)
- `icone_estacao` (icon: PRATICA/HISTORIA/DESAFIO/REVISAO/AVALIACAO)
- `estado` (enum: pendente/concluida)

**Variantes**:
- `pendente`: Card completo com botão "Começar"
- `concluida`: Card com checkmark verde, sem botão

**Comportamento**:
- Tap no card ou botão "Começar" navega ao Player

---

### PlayerFrame
**Onde usa**: Player (Aula Interativa/Story/Missão)
**Props**:
- `lesson_id` (string)
- `beat_current` (number)
- `beats_total` (number)
- `visual_asset` (string URL)
- `narration_text` (string)
- `audio_url` (string, opcional)
- `checkpoint_data` (object)

**Componentes internos**:
- VisualArea (imagem/SVG/animação)
- NarrationText (texto + legendas opcionais)
- CheckpointModule (overlay após narração)
- FeedbackCard (após resposta)
- ProgressBar (bottom, segmentada)

**Comportamento**:
- Toca áudio automaticamente (se `audio_url` presente)
- Exibe CheckpointModule ao fim da narração
- Bloqueia avanço até checkpoint respondido

**Estados**:
- Loading: Skeleton
- Error: "Falha ao carregar beat"
- Success: Layout completo

---

### CheckpointModule
**Onde usa**: Dentro do PlayerFrame
**Props**:
- `type` (enum: ESCOLHA/QUIZ/RECALL/SIMULACAO)
- `prompt` (string)
- `options` (array, se ESCOLHA)
- `correct_answer` (string/number)
- `feedback_correct` (string)
- `feedback_wrong` (string)

**Variantes por Tipo**:
- **ESCOLHA**: Radio buttons, opções múltiplas
- **QUIZ**: Input de texto curto
- **RECALL**: Textarea, resposta aberta
- **SIMULAÇÃO**: Interação customizada (arrastar/clicar)

**Comportamento**:
- Bloqueia avanço até responder
- Micro-feedback visual (shake se errado, glow se certo)
- Exibe FeedbackCard após resposta

**Micro-feedback**:
- Correto: Glow verde suave
- Errado: Shake leve (não punitivo)

[fonte: 07 - alinhamento.md → Motion Designer → Micro-feedback (shake, glow)]

---

### FeedbackCard
**Onde usa**: Após CheckpointModule
**Props**:
- `is_correct` (boolean)
- `feedback_text` (string)
- `identity_insight` (string, opcional) → "o que isso revela sobre você"

**Variantes**:
- `correct`: Checkmark verde, tom encorajador
- `wrong`: Ícone neutro, tom coaching (não punição)

**Comportamento**:
- Exibe feedback que revela algo sobre o aluno
- Botão "Próximo Beat" libera avanço

[fonte: 07 - alinhamento.md → UX Writer → Feedback "o que isso revela"]

---

### ProgressBar (Beats)
**Onde usa**: PlayerFrame (bottom)
**Props**:
- `beat_current` (number)
- `beats_total` (number)

**Visual**: Barra segmentada (ex: 8 segmentos para 8 beats)
**Comportamento**: Atualiza visualmente ao completar beat

---

### SRSCard
**Onde usa**: Review SRS
**Props**:
- `frente` (string)
- `verso` (string)
- `interval_next` (number, dias)

**Estados**:
- `frente_visivel`: Mostra apenas frente + botão "Revelar"
- `verso_visivel`: Mostra verso + botões auto-avaliação (Acertou/Quase/Errou)

**Comportamento**:
- Auto-avaliação ajusta intervalo SM-2

---

### ProofForm (Aplicação) - SIMPLIFICADO
**Onde usa**: Aplicação (Mundo Real)
**Props**:
- `proof_type` (enum: texto/link)
- `checklist_items` (array de strings)

**Variantes**:
- `texto`: Textarea (descrição do que foi feito)
- `link`: Input URL (opcional)
- `checklist`: Lista de checkboxes auto-declarativos

**Comportamento**:
- Validação: Textarea não vazia (min 20 chars) OU URL válida
- Checklist: Todos os items devem ser marcados
- SEM upload de arquivos

**IMPORTANTE**: Upload de arquivos foi REMOVIDO conforme decisão do cliente.
Validação baseada em honestidade + checklist auto-declarativo.

[fonte: Resposta Cliente #2 → REMOVER ou SIMPLIFICAR upload - evitar tarefas de upload]

---

### XPBreakdownCard
**Onde usa**: Conclusão
**Props**:
- `xp_base` (number)
- `bonuses` (array: [{nome, valor}])
- `total` (number)

**Comportamento**:
- Animação de contagem de XP (sutil)
- Transparência total (base + cada bônus nomeado)

---

### BadgeCard
**Onde usa**: Badges/Galeria
**Props**:
- `badge_id` (string)
- `nome` (string)
- `descricao` (string)
- `icon` (string URL)
- `conquistado` (boolean)
- `data_conquista` (date, se conquistado)

**Variantes**:
- `conquistado`: Colorido, exibe data
- `bloqueado`: Grayscale, exibe "Como desbloquear"

**Comportamento**:
- Tap abre modal com detalhe

---

### LeaderboardRow
**Onde usa**: Ligas/Ranking
**Props**:
- `posicao` (number)
- `nome` (string)
- `xp` (number)
- `is_current_user` (boolean)

**Variantes**:
- `current_user`: Destacado (background diferente)
- `top3`: Medalhas (1º/2º/3º)

**Comportamento**:
- Exibe apenas TEAM/PRIVATE (sem exposição pública)

[fonte: 07 - alinhamento.md → Growth/Retention Designer → Ranking TEAM/PRIVATE]

---

## COMPONENTES DO CREATOR STUDIO

### AppCard (Dashboard)
**Onde usa**: Dashboard
**Props**:
- `app_id` (string)
- `titulo` (string)
- `status` (enum: draft/published/qa_failed)
- `usuarios_ativos` (number, se published)
- `rating` (number, se published)

**Variantes**:
- `draft`: Botão "Continuar Build"
- `published`: Botão "Ver Analytics"
- `qa_failed`: Banner vermelho "Corrigir"

**Comportamento**:
- Tap no card abre detalhes
- Botões navegam às respectivas telas

---

### QAGateItem
**Onde usa**: QA Checklist
**Props**:
- `gate_name` (string)
- `status` (enum: ok/warning/error)
- `message` (string)

**Variantes por Status**:
- `ok`: Checkmark verde
- `warning`: Ícone alerta laranja
- `error`: X vermelho + link "Corrigir Agora"

**Comportamento**:
- Se ERROR, exibe banner de bloqueio global
- Link "Corrigir Agora" navega à tela específica

---

### ValidationResultItem (Import Pack)
**Onde usa**: Import Pack
**Props**:
- `file_path` (string)
- `status` (enum: ok/warning/error)
- `message` (string)

**Variantes**: Igual QAGateItem

---

### PreviewEmulator (iFrame)
**Onde usa**: Preview Emulator
**Props**:
- `screen` (enum: home/player/trilha)
- `profile` (enum: SAFE/TENSION/STATUS)
- `device` (enum: mobile/tablet)

**Comportamento**:
- Renderiza componentes do Learner em modo preview
- Dados de draft (não produção)

---

### VersionCard
**Onde usa**: Versioning
**Props**:
- `version` (string, ex: "v1.2")
- `changelog` (string)
- `data_publicacao` (date)
- `is_current` (boolean)

**Variantes**:
- `current`: Badge "Atual"
- `anterior`: Botão "Rollback"

**Comportamento**:
- Rollback exibe modal de confirmação

---

## COMPONENTES DO PLATFORM ADMIN

### WorkspaceRow
**Onde usa**: Tenants/Workspaces
**Props**:
- `workspace_id` (string)
- `nome` (string)
- `usuarios_ativos` (number)
- `plano` (enum: MicroSaaS/Standard/Full)
- `status` (enum: ativo/suspenso)

**Comportamento**:
- Ações admin: Suspender, Contatar, Ver Detalhes

---

### ModerationQueueItem
**Onde usa**: Moderação/Trust
**Props**:
- `tipo` (enum: denuncia/prova)
- `descricao` (string)
- `data` (date)

**Comportamento**:
- Ações: Revisar, Aprovar, Reprovar, Bloquear

---

## COMPONENTES TRANSVERSAIS (Usados em múltiplos apps)

### Button
**Variantes**:
- `primary`: CTA dominante (cor primária, grande)
- `secondary`: Ação secundária (outline)
- `ghost`: Ação terciária (texto apenas)

**Estados**:
- `default`
- `hover`
- `pressed`
- `disabled`
- `loading` (spinner)

**Props**:
- `label` (string)
- `variant` (enum)
- `size` (enum: small/medium/large)
- `disabled` (boolean)
- `loading` (boolean)
- `icon` (component, opcional)

---

### Input (Text/Email/Password)
**Variantes**:
- `text`
- `email` (com validação de formato)
- `password` (com toggle show/hide)
- `textarea`

**Estados**:
- `default`
- `focus`
- `error` (com mensagem de erro)
- `disabled`

**Props**:
- `value` (string)
- `placeholder` (string)
- `type` (enum)
- `error_message` (string, se error)
- `disabled` (boolean)

---

### Modal
**Props**:
- `title` (string)
- `content` (component)
- `actions` (array de botões)

**Comportamento**:
- Overlay escurece fundo
- Fecha com X, ESC ou tap fora (se `dismissable`)

**Acessibilidade**:
- Trap focus dentro do modal
- Anuncia título para screen readers

---

### Dropdown
**Props**:
- `options` (array: [{label, value}])
- `selected` (string/number)
- `placeholder` (string)

**Estados**:
- `closed`
- `open`
- `disabled`

---

### Toggle/Switch
**Props**:
- `checked` (boolean)
- `label` (string)
- `disabled` (boolean)

**Comportamento**:
- Animação suave ao alternar

---

### Checkbox
**Props**:
- `checked` (boolean)
- `label` (string)
- `required` (boolean)
- `disabled` (boolean)

**Estados**:
- `unchecked`
- `checked`
- `indeterminate` (parcialmente marcado, para listas)

---

### Radio Button Group
**Props**:
- `options` (array: [{label, value}])
- `selected` (string/number)
- `name` (string, para agrupamento)

**Comportamento**:
- Apenas 1 selecionável por grupo

---

### Skeleton (Loading State)
**Variantes**:
- `text` (linha de texto)
- `card` (retângulo)
- `circle` (avatar)

**Comportamento**:
- Animação pulse sutil

---

### Toast (Notificação)
**Variantes**:
- `success`: Verde, checkmark
- `error`: Vermelho, X
- `warning`: Laranja, alerta
- `info`: Azul, i

**Props**:
- `message` (string)
- `variant` (enum)
- `duration` (number, ms) → auto-dismiss

**Comportamento**:
- Aparece no topo ou canto
- Auto-dismiss após `duration`
- Pode ser fechado manualmente

---

### Tabs (Tab Bar)
**Props**:
- `tabs` (array: [{label, value}])
- `active` (string/number)

**Comportamento**:
- Underline no tab ativo
- Navegação com teclado (arrows)

---

### Accordion
**Props**:
- `items` (array: [{title, content}])
- `allow_multiple` (boolean) → permite múltiplos abertos

**Comportamento**:
- Expand/collapse com animação
- Ícone chevron (▼/▶)

---

## RESUMO

**Total de Componentes**: 35+

**Por Categoria**:
- Learner: 11 componentes específicos
- Creator Studio: 6 componentes específicos
- Platform Admin: 2 componentes específicos
- Transversais: 16 componentes base

**Princípio**: Componentes são blocos reutilizáveis. Se algo aparece 2x, vira componente.

[fonte: 07 - alinhamento.md → Design Systems Lead → Catálogo finito]

---

**Última Atualização**: 2025-12-26
