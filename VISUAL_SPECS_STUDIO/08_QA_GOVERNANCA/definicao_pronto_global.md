# DEFINITION OF DONE GLOBAL

**DoD para telas, componentes, som, edições**

[fonte: 01_CONSOLIDACAO_CONSELHO.md → PMO → Definition of Done]

---

## DoD PARA TELAS

Uma tela está "pronta" quando:

- [ ] **Layout desenhável no Figma** apenas com spec textual
- [ ] **13 seções do template** preenchidas (Objetivo, Usuário, Layout, Elementos, CTA, Estados, Conteúdo, Som, Eventos, DoD, Modo Studio, Mapeamento Back, Rastreabilidade)
- [ ] **4 estados implementados**: Loading, Empty, Error, Success
- [ ] **1 CTA dominante** definido
- [ ] **Som/Haptics** especificados conforme tension_profile
- [ ] **Eventos Analytics** definidos
- [ ] **Rastreabilidade** cita arquivos fonte
- [ ] **Modo Studio** especifica variantes por edição (MicroSaaS/Standard/Full)

---

## DoD PARA COMPONENTES

Um componente está "pronto" quando:

- [ ] **Props** definidas com tipos
- [ ] **Variantes** especificadas (ex: primary/secondary, SAFE/TENSION/STATUS)
- [ ] **Estados** implementados (default/hover/pressed/disabled/loading)
- [ ] **Comportamento** documentado
- [ ] **Acessibilidade** garantida (focus visível, ARIA labels)
- [ ] **Reutilizável** em múltiplas telas
- [ ] **Catalogado** em `04_INVENTARIO_COMPONENTES/catalogo_componentes.md`

---

## DoD PARA SISTEMA DE SOM

Sistema de som está "pronto" quando:

- [ ] **Mapa por Estado** completo (SAFE/TENSION/STATUS)
- [ ] **Mapa por Ação** completo
- [ ] **Sound Kit v1** com naming conventions
- [ ] **Volume mix** definido (ambiente 20-30%, cues 40-50%, feedback 60-70%, celebração 80-90%)
- [ ] **Acessibilidade** implementada:
  - [ ] Mute global
  - [ ] Toggle som no Player
  - [ ] Reduce sound cues
  - [ ] Haptics ON/OFF
  - [ ] Reduce motion
  - [ ] Legendas no Player

---

## DoD PARA EDIÇÕES (Modo Studio)

Edições estão "prontas" quando:

- [ ] **Matriz de features** completa (MicroSaaS/Standard/Full)
- [ ] **Feature flags** implementados (backend)
- [ ] **Navegação** não mostra links para features desabilitadas
- [ ] **Upsell** implementado (se usuário tenta acessar feature bloqueada)
- [ ] **Quotas** definidas e aplicadas (TTS, storage, usuários, apps)
- [ ] **Jornadas principais** funcionam em TODAS as edições

---

## DoD PARA QA GATES

QA Gates estão "prontos" quando:

- [ ] **6 gates automáticos** validam antes de publish
- [ ] **Bloqueio** funciona (não permite publicar se gate failed)
- [ ] **Warnings** permitem publicar mas alertam
- [ ] **Score de qualidade** calcula corretamente
- [ ] **Threshold marketplace** (≥70) aplicado
- [ ] **Review editorial** por amostra implementado

---

## DoD PARA ANALYTICS

Analytics está "pronto" quando:

- [ ] **Taxonomia de eventos** definida por tela
- [ ] **Eventos críticos** implementados:
  - [ ] `beat_completed` (drop-off por beat)
  - [ ] `lesson_completed` (funil conclusão)
  - [ ] `checkpoint_answered` (validação aprendizagem)
- [ ] **Dashboards-alvo** no Creator Studio:
  - [ ] Funil de conclusão
  - [ ] Drop-off por beat
  - [ ] Cohorts
  - [ ] Heatmaps (Full edition)
- [ ] **Exportar CSV** funcional

---

## DoD PARA DOCUMENTAÇÃO VISUAL_SPECS_STUDIO

Documentação está "pronta" quando:

- [ ] **Todos os arquivos** criados conforme estrutura do README
- [ ] **34+ telas** especificadas (15+ Learner, 13+ Creator Studio, 6+ Platform Admin)
- [ ] **Inventário de Componentes** completo (35+ componentes)
- [ ] **Tokens de Design** definidos (placeholders para designer popular)
- [ ] **Sistema de Som** completo (3 arquivos)
- [ ] **Analytics Light** taxonomia definida
- [ ] **QA/Governança** gates e DoD documentados
- [ ] **Manifesto JSON** legível por máquina
- [ ] **Questões Abertas** identificadas

---

## CHECKLIST FINAL (Antes de Desenvolvimento)

Antes de iniciar desenvolvimento:

- [ ] **Alignment**: Times alinhados com specs
- [ ] **Design System**: Figma com componentes/tokens populados
- [ ] **Backend Contracts**: APIs definidas (não implementadas, apenas contratos)
- [ ] **Feature Flags**: Sistema de edições preparado
- [ ] **Analytics SDK**: Integrado (ex: Segment, Amplitude)
- [ ] **QA Gates**: Validações server-side prontas

---

**Última Atualização**: 2025-12-26
