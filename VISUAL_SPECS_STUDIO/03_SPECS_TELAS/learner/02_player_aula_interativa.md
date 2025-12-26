# PLAYER (AULA INTERATIVA)

## 1) Objetivo da Tela
Entregar a experiência de aprendizagem ativa através de beats estruturados (45-90s cada) com checkpoints obrigatórios. O Player é o coração do produto: transforma consumo passivo em processamento ativo.

[fonte: 01 - economia.md → Neuro/Aprendizagem → Aula Interativa em Beats]

## 2) Usuário & Contexto
**Usuário**: Learner (aluno) logado
**Contexto**: Após clicar "Começar" ou "Continuar" na Home. O aluno está focado em aprender um conceito específico.
**Frequência de uso**: Diária (core loop principal)
**Duração média**: 8-12 minutos (8 beats × 45-90s)

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.1 Learner App → Conteúdo → Player]

## 3) Layout & Hierarquia (wireframe textual)

```
┌─────────────────────────────────────┐
│ [TopBar - Controles]                │
│ [X Sair] [Progresso: Beat 3/8] [🔊] │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Visual Area]                       │
│                                     │
│   [Imagem/SVG/Animação]             │
│   (Asset do beat atual)             │
│                                     │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Narração/Texto]                    │
│                                     │
│ "A Big Idea é a promessa com        │
│  mecanismo: algo novo e crível."    │
│                                     │
│ [Legendas: ON/OFF]                  │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [CheckpointModule - Overlay]        │
│ (Aparece após narração do beat)     │
│                                     │
│ "Qual opção define melhor Big Idea?"│
│                                     │
│ [○] Uma promessa genérica           │
│ [○] Promessa com mecanismo crível   │
│ [○] Um título bonito                │
│                                     │
│ [Botão: "Responder"]                │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Feedback Card - Após resposta]     │
│                                     │
│ ✓ "Boa. Você entendeu o núcleo."    │
│                                     │
│ "Isso mostra que você identifica    │
│  padrões, não aceita genéricos."    │
│                                     │
│ [Botão: "Próximo Beat"]             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [ProgressBar - Bottom]              │
│ ▓▓▓░░░░░ (Beat 3 de 8 concluído)    │
└─────────────────────────────────────┘
```

**Hierarquia Visual**:
1. Visual Area (imagem/svg)
2. Narração/Texto
3. CheckpointModule (bloqueia avanço)
4. FeedbackCard (após resposta)
5. ProgressBar (sempre visível no bottom)

[fonte: 03 - todas as telas, flows e governança fechados.md → Player unificado com modes]

## 4) Elementos & Componentes (comportamento)

### PlayerFrame (Container)
- **Props**: `lesson_id`, `beat_current`, `beats_total`, `visual_asset`, `narration_text`, `checkpoint_data`
- **Comportamento**:
  - Exibe beat atual
  - Toca áudio (narração) automaticamente
  - Mostra CheckpointModule ao fim da narração
  - Avança para próximo beat apenas após checkpoint respondido

[fonte: 04_INVENTARIO_COMPONENTES/catalogo_componentes.md → PlayerFrame]

### Visual Area
- **Tipo**: Imagem/SVG/Animação (conforme asset do beat)
- **Comportamento**: Estático durante narração; pode ter animação leve (fade-in)

### Narração/Texto
- **Props**: `text`, `audio_url`
- **Comportamento**:
  - Áudio toca automaticamente (com toggle de som)
  - Texto exibido sincronizado com áudio
  - Legendas opcionais (toggle)

[fonte: 07 - alinhamento.md → Accessibility → Legendas no player]

### CheckpointModule (Componente Crítico)
- **Tipos**: ESCOLHA / QUIZ / RECALL / SIMULAÇÃO
- **Props**: `type`, `prompt`, `options` (se escolha), `correct_answer`, `feedback_correct`, `feedback_wrong`
- **Comportamento**:
  - Aparece automaticamente ao fim da narração
  - Bloqueia avanço até responder
  - Exibe FeedbackCard após resposta
  - Micro-feedback visual (shake se errado, glow se certo)

[fonte: 01 - economia.md → Neuro/Aprendizagem → Checkpoint obrigatório]

### FeedbackCard
- **Props**: `is_correct`, `feedback_text`, `identity_insight`
- **Comportamento**:
  - Exibe feedback que revela algo sobre o aluno (não apenas "certo/errado")
  - Botão "Próximo Beat" libera avanço

[fonte: 07 - alinhamento.md → UX Writer → Feedback "o que isso revela"]

### ProgressBar
- **Props**: `beat_current`, `beats_total`
- **Visual**: Barra segmentada (8 segmentos para 8 beats)
- **Comportamento**: Atualiza ao completar beat

[fonte: 03 - todas as telas, flows e governança fechados.md → Progress Bar (beats)]

### TopBar Controles
- **Botão Sair**: Exibe modal "Tem certeza? Seu progresso será salvo."
- **Toggle Som**: Mute/unmute (ícone 🔊/🔇)
- **Progresso**: "Beat [N]/8"

## 5) Ação Primária (1 CTA dominante)

**CTA Dominante varia conforme estado**:
- Durante narração: Nenhum (usuário escuta/lê)
- Checkpoint visível: **"Responder"**
- Feedback exibido: **"Próximo Beat"**

**Regra**: Apenas 1 CTA visível por vez. O fluxo é sequencial.

[fonte: 03 - todas as telas, flows e governança fechados.md → 1 CTA dominante por tela]

## 6) Estados (obrigatórios: Loading, Empty, Error, Success)

### Loading
- **Quando**: Ao carregar lesson e assets do primeiro beat
- **Visual**: Skeleton do PlayerFrame
- **Duração**: < 2s

### Empty
- **Não aplicável** (Player sempre tem conteúdo ao abrir)

### Error
- **Quando**: Falha ao carregar beat ou áudio
- **Visual**: Ícone de erro + mensagem "Não conseguimos carregar este beat. Verifique sua conexão."
- **CTA**: "Tentar novamente" ou "Voltar à Home"

### Success
- **Quando**: Beat carregado e checkpoint respondido com sucesso
- **Visual**: Layout completo conforme descrito

## 7) Conteúdo / Microcopy

### Narração/Texto
- Texto curto e claro (otimizado para áudio)
- Exemplo: "A Big Idea é a promessa com mecanismo: algo novo e crível."

### CheckpointModule
- Prompt: Pergunta clara e direta
- Exemplo: "Qual opção define melhor Big Idea?"
- Opções: Curtas, claras, sem ambiguidade

### FeedbackCard
- **Se correto**: "Boa. Você entendeu o núcleo." + Insight: "Isso mostra que você identifica padrões, não aceita genéricos."
- **Se errado**: "Isso é promessa solta. Falta mecanismo." + Coaching: "Big Idea = Promessa + Mecanismo crível."

**Tom**: Profissional, coaching, sem punição.

[fonte: 07 - alinhamento.md → UX Writer → Texto curto + claro por beat]

### Modal Sair
- "Tem certeza que quer sair?"
- "Seu progresso será salvo no beat atual."
- Botões: "Cancelar" / "Sair"

## 8) Som/Haptics (porquê + quando - mapeamento SAFE/TENSION/STATUS)

### Tension Profile: SAFE ou TENSION (conforme estação)

[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile]

### Sons

| Ação | Som | Quando | Por Quê |
|------|-----|--------|---------|
| Iniciar Player | `ambient_safe.mp3` ou `ambient_tension.mp3` | Ao carregar | Estabelece clima conforme estação |
| Narração | `[audio_url]` | Automaticamente | Conteúdo principal |
| Checkpoint correto | `correct_soft.mp3` (SAFE) ou `correct_tension.mp3` (TENSION) | Ao responder certo | Reforço positivo |
| Checkpoint errado | `wrong_soft.mp3` | Ao responder errado | Coaching, não punição |
| Próximo beat | `transition_snap.mp3` | Ao avançar beat | Sensação de progresso |

**Volume**: Médio (narração), Baixo (ambiente/cues)

[fonte: 06_SISTEMA_SOM/mapa_som_por_estado.md → SAFE/TENSION]

### Haptics

| Ação | Haptic | Quando |
|------|--------|--------|
| Checkpoint correto | Success (médio) | Ao responder certo |
| Checkpoint errado | Light impact | Ao responder errado (sem punição) |
| Próximo beat | Light impact | Ao avançar |

[fonte: 06_SISTEMA_SOM/regras_acessibilidade.md → Haptics]

## 9) Eventos (Analytics LIGHT)

| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `player_opened` | Tela carregada | `user_id`, `lesson_id`, `beat_start` |
| `beat_viewed` | Beat exibido | `user_id`, `lesson_id`, `beat_number`, `timestamp` |
| `checkpoint_answered` | Resposta enviada | `user_id`, `lesson_id`, `beat_number`, `checkpoint_type`, `is_correct`, `time_to_answer` |
| `beat_completed` | Avança para próximo beat | `user_id`, `lesson_id`, `beat_number`, `duration` |
| `player_exited` | Sai do Player | `user_id`, `lesson_id`, `beat_current`, `exit_reason` (user/error) |

**Crítico para Analytics**: `checkpoint_answered` e `beat_completed` geram funil de drop-off por beat.

[fonte: 07_ANALYTICS_LIGHT/taxonomia_eventos_por_tela.md]

## 10) Definition of Done para esta tela

- [ ] Layout desenhável no Figma apenas com esta spec
- [ ] PlayerFrame carrega beats sequencialmente
- [ ] Narração toca automaticamente (com toggle de som)
- [ ] CheckpointModule bloqueia avanço até responder
- [ ] FeedbackCard exibe feedback com insight (não apenas certo/errado)
- [ ] ProgressBar atualiza visualmente por beat
- [ ] 3 estados implementados (Loading/Error/Success)
- [ ] Sons SAFE/TENSION implementados conforme estação
- [ ] Micro-feedback visual (shake/glow) no checkpoint
- [ ] Legendas opcionais funcionais
- [ ] Eventos Analytics implementados (funil por beat)

[fonte: 08_QA_GOVERNANCA/definicao_pronto_global.md → DoD para telas]

## 11) Modo Studio / Edições (MicroSaaS vs Full)

### MicroSaaS (low-ticket)
- **Mantém**: PlayerFrame, CheckpointModule, FeedbackCard, ProgressBar
- **Remove**: Legendas opcionais (apenas áudio)
- **Limita**: Máximo 5 beats por aula (vs 8 no Full)

### Standard
- **Mantém**: Tudo do MicroSaaS
- **Adiciona**: Legendas opcionais
- **Expande**: Até 8 beats por aula

### Full
- **Mantém**: Tudo
- **Adiciona**: Analytics detalhado por beat (heatmaps, time-to-answer)

[fonte: 05_VARIANTES_MODO_STUDIO/matriz_edicoes.md]

## 12) Mapeamento Back Após Visual (NÃO IMPLEMENTAR)

**Este é orientação para implementação futura, não spec funcional.**

### Inputs de Dados
- `GET /api/learner/lesson/:id/beats`
  - Retorna: `beats[]` (array com `beat_number`, `visual_asset`, `narration_text`, `audio_url`, `checkpoint`)

### Outputs de Ações
- `POST /api/learner/checkpoint/answer`
  - Payload: `lesson_id`, `beat_number`, `checkpoint_type`, `answer`, `is_correct`, `time_to_answer`
- `POST /api/learner/beat/complete`
  - Payload: `lesson_id`, `beat_number`, `duration`

### Integrações Sugeridas
- **Audio Player**: `react-native-sound` ou `expo-av` (narração)
- **Progress Tracking**: State local (Zustand) + sync com backend
- **Checkpoint Validation**: Lógica no frontend, log no backend

### Atalhos Recomendados
- **PlayerFrame**: Componente reutilizável (Aula Interativa, Story, Missão usam o mesmo)
- **CheckpointModule**: Biblioteca de tipos (ESCOLHA/QUIZ/RECALL/SIMULAÇÃO) reutilizável
- **FeedbackCard**: Template com variantes (SAFE/TENSION/STATUS)

### Observações de Performance
- **Cache de assets**: Pré-carregar assets dos próximos 2 beats
- **Áudio**: Streaming (não download completo antes de iniciar)
- **Checkpoint**: Validação local (não esperar backend para avançar)

### Observações de Custo (ElevenLabs TTS)
- **Cache de áudio**: Armazenar resultado TTS (não regerar)
- **Quotas**: Validar limite de TTS por plano antes de gerar

[fonte: 01_CONSOLIDACAO_CONSELHO.md → FinOps/Infra Cost Lead → Caching + limites]

## 13) Rastreabilidade

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.1 Learner App → Conteúdo → Player]
[fonte: 01 - economia.md → Neuro/Aprendizagem → Aula Interativa em Beats]
[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile SAFE/TENSION]
[fonte: 07 - alinhamento.md → Motion/Interaction Designer → Beats + checkpoints como ritmo do produto]
[fonte: 07 - alinhamento.md → UX Writer → Feedback "o que isso revela"]
[fonte: 07 - alinhamento.md → Accessibility → Legendas no player]

---

**Última Atualização**: 2025-12-26
