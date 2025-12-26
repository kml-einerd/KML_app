# CONCLUSÃO (XP BREAKDOWN)

## 1) Objetivo da Tela
Celebrar a conclusão da aula de forma clara e profissional, exibindo breakdown transparente de XP (base + bônus) e reforçando progresso. Prepara transição para próxima ação sem infantilizar.

[fonte: 03 - todas as telas, flows e governança fechados.md → Componentes UI → XP Breakdown Modal]

## 2) Usuário & Contexto
**Usuário**: Learner (aluno) após completar aula
**Contexto**: Final do Player, após último beat
**Frequência**: Após cada aula (3-5x por semana)

## 3) Layout & Hierarquia

```
┌─────────────────────────────────────┐
│ [Ícone Conclusão]                   │
│ ✓ Aula Concluída                    │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [XP Breakdown]                      │
│                                     │
│ XP Base: +60                        │
│ Streak (7 dias): +6 (×1.1)          │
│ Checkpoints 100%: +10               │
│ ────────────────                    │
│ Total: +76 XP                       │
│                                     │
│ [Barra de Progresso]                │
│ Nível 5: 476/500 XP                 │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Resumo da Aula]                    │
│ "Você aprendeu:"                    │
│ • Big Idea = Promessa + Mecanismo   │
│ • Mecanismo crível quebra ceticismo │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Ação Aplicada]                     │
│ "Próximo passo:"                    │
│ "Escreva 1 Big Idea do seu tema     │
│  com promessa + mecanismo."         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [CTA Primário]                      │
│ [Botão: "Continuar"]                │
└─────────────────────────────────────┘
```

[fonte: 01 - economia.md → Neuro/Aprendizagem → Finalização = resumo + ação aplicada]

## 4) Elementos & Componentes

- **XP Breakdown Card**: `xp_base`, `bonuses[]`, `total`
- **ProgressBar**: Atualiza com novo XP
- **Resumo**: Bullets com takeaways principais
- **Ação Aplicada**: Tarefa concreta para mundo real
- **Confetti** (condicional): Apenas se level-up ou badge unlock

[fonte: 07 - alinhamento.md → Motion Designer → Confetti condicional]

## 5) Ação Primária
**"Continuar"** → Volta à Home

## 6) Estados
- **Loading**: Calculando XP (< 1s)
- **Success**: Breakdown exibido
- **Error**: "Erro ao salvar progresso" + retry

## 7) Conteúdo / Microcopy
- Título: "Aula Concluída" (não "Parabéns!!!")
- XP: Transparente (base + cada bônus nomeado)
- Resumo: Objetivos em bullets
- Ação: Verbo imperativo claro

[fonte: 07 - alinhamento.md → UX Writer → Remover tom infantil]

## 8) Som/Haptics

| Ação | Som | Quando |
|------|-----|--------|
| Abrir Conclusão | `complete_success.mp3` | Ao carregar |
| XP Breakdown | `xp_count.mp3` (sutil) | Durante animação |
| Level-up | `level_up_celebration.mp3` | Se subiu de nível |
| Confetti | `confetti.mp3` | Se level-up/badge |

**Tension Profile**: STATUS (validação, orgulho)

[fonte: 06_SISTEMA_SOM/mapa_som_por_estado.md → STATUS]

## 9) Eventos

| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `lesson_completed` | Tela carregada | `user_id`, `lesson_id`, `xp_earned`, `bonuses_applied`, `new_level` |
| `continue_clicked` | Tap "Continuar" | `user_id` |

## 10) Definition of Done
- [ ] XP Breakdown transparente (base + bônus)
- [ ] ProgressBar atualiza visualmente
- [ ] Resumo + Ação Aplicada exibidos
- [ ] Confetti apenas se level-up/badge
- [ ] Som STATUS implementado
- [ ] Eventos Analytics implementados

## 11) Modo Studio / Edições
- **MicroSaaS**: XP Breakdown simplificado (sem bônus)
- **Standard/Full**: Breakdown completo

## 12) Mapeamento Back Após Visual
- `POST /api/learner/lesson/complete`
  - Retorna: `xp_breakdown`, `new_level`, `badges_unlocked`

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → XP Breakdown Modal]
[fonte: 01 - economia.md → Game Economy → XP base por arquétipo]

---

**Última Atualização**: 2025-12-26
