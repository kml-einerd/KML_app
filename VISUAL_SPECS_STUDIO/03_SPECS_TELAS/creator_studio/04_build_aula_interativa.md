# BUILD AULA INTERATIVA

## 1) Objetivo da Tela
Interface para construir Aula Interativa manualmente (alternativa ao import): beats + checkpoints + feedback.

[fonte: 03 - todas as telas, flows e governança fechados.md → Build por Formato → Build Aula Interativa]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer construir aula manualmente, **Frequência**: Ocasional (import é preferido)

## 3) Layout & Hierarquia
```
[Header: "Build Aula Interativa"]
Título: [_______________________]
Estação: [Dropdown: SAFE/TENSION/STATUS]
XP Base: [60] (sugestão: 50-80)

[Beats Editor - Accordion]
▼ Beat 1 de 8
  Narração: [Textarea]
  Visual: [Upload SVG/Imagem]
  Audio (TTS): [Gerar com ElevenLabs]

  Checkpoint:
    Tipo: [ESCOLHA/QUIZ/RECALL/SIMULAÇÃO]
    Prompt: [_____]
    Opções: [____] [____] [____]
    Resposta Correta: [2]
    Feedback Correto: [____]
    Feedback Errado: [____]

▶ Beat 2 de 8
▶ Beat 3 de 8
...

[Botões]
[Salvar Draft] [Preview] [Publicar]
```

## 4) Elementos & Componentes
- **Form Fields**: Título, Estação, XP, Beats (accordion)
- **TTS Integration**: Botão "Gerar Áudio" (ElevenLabs)
- **Checkpoint Builder**: Form dinâmico conforme tipo

[fonte: 01 - economia.md → Neuro/Aprendizagem → Beats: 8 beats de 45-90s]

## 5) Ação Primária
"Salvar Draft" (durante construção) ou "Preview" (antes de publicar)

## 6) Estados
- **Loading**: Salvando ou gerando TTS
- **Error**: "Beat sem checkpoint", "Campo obrigatório vazio"
- **Success**: "Aula salva com sucesso"

## 7) Conteúdo / Microcopy
- "Adicionar Beat"
- "Gerar Áudio (TTS)" → mostra custo/quota
- Validação: "Todo beat deve ter checkpoint"

[fonte: 05 - sistema completo de EdTech.md → Gate 1: Atividade]

## 8) Som/Haptics
**SAFE**: `save_success.mp3`

## 9) Eventos
`lesson_build_started`, `beat_added`, `tts_generated`, `lesson_saved`

## 10) Definition of Done
- [ ] Form permite criar 8 beats
- [ ] Cada beat obriga checkpoint
- [ ] TTS integration funcional (ElevenLabs)
- [ ] Validação bloqueia salvar se beat sem checkpoint
- [ ] Preview disponível

## 11) Modo Studio / Edições
- **MicroSaaS**: Até 5 beats (vs 8 no Standard/Full)
- **Standard/Full**: Até 8 beats

## 12) Mapeamento Back
`POST /api/creator/lesson/create`, `POST /api/creator/lesson/:id/beat`
TTS: Integração ElevenLabs API (cache resultado)

Atalho: Template de form reutilizável para checkpoints

[fonte: 01_CONSOLIDACAO_CONSELHO.md → FinOps → TTS quota por plano]

## 13) Rastreabilidade
[fonte: 04 - modo Studio.md → Especificação mínima do arquivo de Aula Interativa]
[fonte: 01 - economia.md → Neuro/Aprendizagem → Beats + Checkpoints]

---
