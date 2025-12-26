# HOME (MISSÃO DO DIA)

## 1) Objetivo da Tela
Apresentar ao aluno a missão principal do dia de forma clara e imediata, reduzindo fricção entre login e início da atividade. A tela serve como ponto de partida diário e reforça o hábito através de streak visual e clareza de "o que fazer agora".

[fonte: 03 - todas as telas, flows e governança fechados.md → Core Diário → Home (Missão do Dia)]

## 2) Usuário & Contexto
**Usuário**: Learner (aluno) logado
**Contexto**: Primeira tela após login ou ao abrir o app. O aluno quer saber "o que fazer agora" sem pensar.
**Frequência de uso**: Diária (principal porta de entrada)

[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 1.2 Seções e Telas do Learner → CORE DIÁRIO]

## 3) Layout & Hierarquia (wireframe textual)

```
┌─────────────────────────────────────┐
│ [Header]                            │
│ ProgressHeader                      │
│ - Avatar + Nome                     │
│ - Streak: 🔥 7 dias                 │
│ - XP: nível 5 (450/500)             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [CTA Primário - DOMINANTE]          │
│                                     │
│  [Botão "Continuar"]                │
│  "Aula Interativa: Big Idea"        │
│  Beat 3 de 8 • 12 min restantes     │
│                                     │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Missão do Dia]                     │
│ MissionCard                         │
│ - Ícone da estação (⚡ Prática)     │
│ - Título: "Criar sua primeira Big Idea" │
│ - XP: +60                           │
│ - Estado: pendente                  │
│                                     │
│ [Botão Secundário: "Começar"]       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Review SRS - Se houver itens]     │
│ "3 itens para revisar hoje"         │
│ [Link: "Revisar agora"]             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Bottom Tabs - Navegação]           │
│ Home • Trilha • Review • Progresso • Perfil │
└─────────────────────────────────────┘
```

**Hierarquia Visual**:
1. ProgressHeader (identidade + streak)
2. Botão "Continuar" (DOMINANTE - se houver atividade em andamento)
3. MissionCard (se não houver atividade em andamento)
4. Review SRS (secundário, se houver)
5. Bottom Tabs (navegação)

[fonte: 03 - todas as telas, flows e governança fechados.md → 1 CTA dominante por tela]

## 4) Elementos & Componentes (comportamento)

### ProgressHeader
- **Props**: avatar, nome, streak_dias, xp_atual, xp_proximo_nivel, nivel
- **Comportamento**:
  - Streak em risco (< 24h para quebrar) mostra alerta laranja
  - Tap no streak abre modal explicando sistema
  - Progresso XP é barra visual

[fonte: 07 - alinhamento.md → Growth/Retention Designer → Streak com governança]

### Botão "Continuar" (Condicional)
- **Exibe se**: Existe atividade em andamento (aula não concluída)
- **Comportamento**: Leva diretamente ao Player no beat onde parou
- **Estado**: Primário, grande, posição dominante

[fonte: 07 - alinhamento.md → UX Architect → "Continuar" sempre visível]

### MissionCard
- **Props**: titulo, descricao, xp, icone_estacao, estado (pendente/concluida)
- **Estados**:
  - `pendente`: Card completo com botão "Começar"
  - `concluida`: Card com checkmark verde, sem botão
- **Comportamento**: Tap no card ou botão "Começar" abre Player

[fonte: 03 - todas as telas, flows e governança fechados.md → Componentes UI obrigatórios → Mission Card]

### Review SRS Widget (Condicional)
- **Exibe se**: Existem itens para revisar hoje (agenda SM-2)
- **Comportamento**: Link leva à tela Review SRS
- **Visual**: Badge com número de itens

[fonte: 01 - economia.md → Cientista Comportamental → Revisão estrutural]

### Bottom Tabs
- **Tabs**: Home (ativo) • Trilha • Review • Progresso • Perfil
- **Comportamento**: Navegação entre seções principais

[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 1.4 Navegação Bottom Tabs]

## 5) Ação Primária (1 CTA dominante)

**CTA Dominante**:
- Se há atividade em andamento: **"Continuar"** (botão grande, cor primária)
- Se não há atividade em andamento: **"Começar"** (na MissionCard)

**Regra**: Apenas 1 botão dominante visível. O outro fica secundário ou oculto.

[fonte: 03 - todas as telas, flows e governança fechados.md → 1 CTA dominante por tela]

## 6) Estados (obrigatórios: Loading, Empty, Error, Success)

### Loading
- **Quando**: Ao carregar missão do dia e progresso
- **Visual**: Skeleton do ProgressHeader + MissionCard
- **Duração**: < 1s

### Empty
- **Quando**: Aluno completou todas as missões disponíveis
- **Visual**: Ilustração + mensagem "Você completou tudo! Volte amanhã."
- **CTA**: "Explorar trilha" ou "Revisar conteúdo"

### Error
- **Quando**: Falha ao carregar missão do dia
- **Visual**: Ícone de erro + mensagem "Não conseguimos carregar sua missão. Tente novamente."
- **CTA**: "Tentar novamente"

### Success
- **Quando**: Estado padrão - missão carregada com sucesso
- **Visual**: Layout completo conforme descrito

[fonte: 04_INVENTARIO_COMPONENTES/estados_e_variantes.md → Estados padrão]

## 7) Conteúdo / Microcopy

### ProgressHeader
- Streak: "🔥 [N] dias" ou "🔥 Em risco!" (se < 24h)
- XP: "Nível [N] • [atual]/[próximo] XP"

### Botão "Continuar"
- Texto: "Continuar"
- Subtítulo: "[Nome da Aula] • Beat [N] de 8 • [X] min restantes"

### MissionCard
- Título: Objetivo claro (ex: "Criar sua primeira Big Idea")
- XP: "+[N] XP"
- Botão: "Começar"

### Review SRS
- "[N] itens para revisar hoje"
- Link: "Revisar agora"

### Empty State
- "Parabéns! Você completou tudo disponível hoje."
- "Volte amanhã para novas missões ou explore a trilha completa."

### Error State
- "Ops! Não conseguimos carregar sua missão."
- "Verifique sua conexão e tente novamente."

**Tom**: Profissional, encorajador, sem infantilização.

[fonte: 07 - alinhamento.md → UX Writer → Remover tom infantil]

## 8) Som/Haptics (porquê + quando - mapeamento SAFE/TENSION/STATUS)

### Tension Profile: SAFE
A Home é sempre SAFE (sem pressão, controle total).

[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile SAFE]

### Sons

| Ação | Som | Quando | Por Quê |
|------|-----|--------|---------|
| Abrir Home | `ambient_safe.mp3` | Ao carregar tela | Estabelece clima seguro |
| Tap "Continuar" ou "Começar" | `start_soft.mp3` | Ao clicar CTA | Reforço positivo de início |
| Streak em risco (visual) | `alert_soft.mp3` | Ao exibir alerta | Aviso gentil, não punitivo |

**Volume**: Baixo (ambiente)
**Acessibilidade**: Respeita toggle global de som

[fonte: 06_SISTEMA_SOM/mapa_som_por_estado.md → SAFE]

### Haptics

| Ação | Haptic | Quando |
|------|--------|--------|
| Tap CTA primário | Light impact | Ao tocar botão |
| Streak em risco | Warning (suave) | Ao exibir alerta |

**Acessibilidade**: Respeita toggle de haptics

[fonte: 06_SISTEMA_SOM/regras_acessibilidade.md → Haptics]

## 9) Eventos (Analytics LIGHT)

| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `home_viewed` | Tela carregada | `user_id`, `streak_dias`, `xp_nivel`, `has_continue_button` |
| `continue_clicked` | Tap "Continuar" | `user_id`, `lesson_id`, `beat_current` |
| `mission_started` | Tap "Começar" em MissionCard | `user_id`, `mission_id`, `xp_value` |
| `review_srs_clicked` | Tap "Revisar agora" | `user_id`, `items_count` |

[fonte: 07_ANALYTICS_LIGHT/taxonomia_eventos_por_tela.md]

## 10) Definition of Done para esta tela

- [ ] Layout desenhável no Figma apenas com esta spec
- [ ] ProgressHeader com streak e XP funcional
- [ ] Botão "Continuar" aparece apenas se há atividade em andamento
- [ ] MissionCard exibe missão do dia com estado correto
- [ ] 4 estados implementados (Loading/Empty/Error/Success)
- [ ] Som SAFE implementado com toggle de acessibilidade
- [ ] Eventos Analytics LIGHT implementados
- [ ] 1 CTA dominante visível por vez
- [ ] Bottom Tabs navegam corretamente

[fonte: 08_QA_GOVERNANCA/definicao_pronto_global.md → DoD para telas]

## 11) Modo Studio / Edições (MicroSaaS vs Full)

### MicroSaaS (low-ticket)
- **Mantém**: ProgressHeader, MissionCard, Botão Continuar, Bottom Tabs
- **Remove**: Review SRS Widget (feature Full)
- **Simplifica**: Streak sem notificações push (apenas visual)

### Standard
- **Mantém**: Tudo do MicroSaaS
- **Adiciona**: Review SRS Widget

### Full
- **Mantém**: Tudo
- **Adiciona**: Notificações push para streak em risco

[fonte: 05_VARIANTES_MODO_STUDIO/matriz_edicoes.md]

## 12) Mapeamento Back Após Visual (NÃO IMPLEMENTAR)

**Este é orientação para implementação futura, não spec funcional.**

### Inputs de Dados
- `GET /api/learner/home`
  - Retorna: `mission_of_day`, `continue_activity`, `streak`, `xp`, `review_srs_count`

### Outputs de Ações
- `POST /api/learner/activity/start` (ao clicar "Começar" ou "Continuar")
  - Payload: `activity_id`, `type` (mission/lesson/review)

### Integrações Sugeridas
- **State Management**: Zustand ou Jotai (leve, sem boilerplate)
- **Cache**: React Query (cache automático de `GET /home`)
- **Navegação**: React Navigation (bottom tabs)

### Atalhos Recomendados
- **ProgressHeader**: Usar biblioteca `react-native-progress` para barra XP
- **MissionCard**: Componente reutilizável (definir em Design System)
- **Bottom Tabs**: Template do React Navigation (customizar estilo)

### Observações de Performance
- Cache da missão do dia por 1h (evita refetch desnecessário)
- Streak e XP devem atualizar em tempo real (WebSocket ou polling leve)

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Atalhos via bibliotecas]

## 13) Rastreabilidade

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.1 Learner App → Core Diário → Home (Missão do Dia)]
[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 1.2 Seções e Telas do Learner → CORE DIÁRIO]
[fonte: 07 - alinhamento.md → UX Architect → "Continuar" sempre visível]
[fonte: 07 - alinhamento.md → Growth/Retention Designer → Streak com governança]
[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile SAFE]
[fonte: 03 - todas as telas, flows e governança fechados.md → 1 CTA dominante por tela]

---

**Última Atualização**: 2025-12-26
