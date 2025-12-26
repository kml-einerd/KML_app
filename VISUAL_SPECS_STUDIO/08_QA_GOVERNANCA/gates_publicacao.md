# GATES DE PUBLICAÇÃO

**Gates objetivos que bloqueiam publicação de apps ruins**

[fonte: 05 - sistema completo de EdTech.md → Quality Gates automáticos]

---

## GATES OBRIGATÓRIOS (Bloqueiam Publicação)

### Gate 1: ATIVIDADE
**Regra**: Aula tem checkpoint em TODO beat
**Validação**: Se `beat.checkpoint == null` → BLOQUEIA
**Mensagem**: "Beat [N] da Aula '[Título]' não tem checkpoint. Adicione checkpoint antes de publicar."
**Por quê**: Checkpoint = processamento ativo (core do produto)

[fonte: 01 - economia.md → Neuro/Aprendizagem → Checkpoint obrigatório]

---

### Gate 2: TRANSFERÊNCIA
**Regra**: Cada módulo tem pelo menos 1 Aplicação
**Validação**: Se `modulo.applications.count == 0` → BLOQUEIA (ou WARNING)
**Mensagem**: "Módulo '[Nome]' não tem Aplicação. Adicione pelo menos 1 aplicação com prova."
**Por quê**: Aplicação = transferência pro mundo real

---

### Gate 3: PROGRESSÃO
**Regra**: Trilha tem pré-requisitos claros/ordem
**Validação**: Se `trilha.modules` sem ordem → BLOQUEIA
**Mensagem**: "Trilha não define ordem de módulos. Configure pré-requisitos."
**Por quê**: Progressão clara evita confusão

---

### Gate 4: FEEDBACK
**Regra**: Todo checkpoint tem feedback (não só "certo/errado")
**Validação**: Se `checkpoint.feedback_correct == null` → BLOQUEIA
**Mensagem**: "Checkpoint do Beat [N] não tem feedback. Adicione feedback que revele algo sobre o aluno."
**Por quê**: Feedback coaching, não punição

[fonte: 07 - alinhamento.md → UX Writer → Feedback "o que isso revela"]

---

### Gate 5: TEMPO/ESCOPO
**Regra**: Beats 45-90s (tolerância: ±10s)
**Validação**: Se `beat.duration < 35s` ou `> 100s` → WARNING (não bloqueia, apenas alerta)
**Mensagem**: "Beat [N] tem [X]s (recomendado: 45-90s). Considere ajustar."
**Por quê**: Ritmo ideal de aprendizagem

[fonte: 01 - economia.md → Neuro/Aprendizagem → Beats 45-90s]

---

### Gate 6: CLAREZA
**Regra**: Objetivo da aula + "o que você saberá fazer"
**Validação**: Se `lesson.learning_outcome == null` → BLOQUEIA
**Mensagem**: "Aula '[Título]' não define objetivo de aprendizagem. Adicione 'o que o aluno saberá fazer'."
**Por quê**: Clareza de outcome

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Editorial Lead → Objetivos obrigatórios]

---

## SCORE DE QUALIDADE (Para Marketplace)

Apps com score abaixo de threshold ficam:
- "Não listados" no marketplace
- Ou com selo "Beta"
- Ou exigem revisão manual editorial

### Dimensões do Score

| Dimensão | Como Medir | Alvo | Peso |
|----------|------------|------|------|
| Atividade | % beats com checkpoint | 100% | 30% |
| Transferência | Apps com prova/módulo | ≥1 | 20% |
| Profundidade | Nº simulações/aplicações por trilha | Mínimo por nível | 15% |
| Retenção | SRS presente (se faz sentido) | Sim/Não | 10% |
| Clareza | Objetivo + resumo + ação aplicada | 100% | 15% |
| Engajamento Real | Completion rate + retorno D1/D7 | Thresholds | 10% |

**Score Total**: 0-100
**Threshold Marketplace**: ≥ 70

[fonte: 05 - sistema completo de EdTech.md → Score de Qualidade]

---

## PADRÕES EDITORIAIS (Review Manual por Amostra)

Além de gates automáticos, apps listados em marketplace passam por review editorial (5% amostra ou todos apps públicos).

### Checklist Editorial

- [ ] Learning outcomes claros
- [ ] 1 exemplo real por aula (mínimo)
- [ ] Aplicação sempre existir
- [ ] Rubrica com critérios objetivos
- [ ] Linguagem clara (clareza > jargão)
- [ ] Tension dial coerente com estação

[fonte: 05 - sistema completo de EdTech.md → Padrões editoriais]

---

## RESUMO: FLUXO DE VALIDAÇÃO

```
[Creator → QA Checklist]
  ↓
[Gates Automáticos]
  ├─ Gate 1-4 FAILED → BLOQUEIA
  ├─ Gate 5 WARNING → Alerta (pode publicar)
  └─ Gate 6 FAILED → BLOQUEIA
  ↓
[Se TUDO OK]
  ↓
[Publish]
  ↓
[Score de Qualidade]
  ├─ Score < 70 → "Não listado" ou "Beta"
  └─ Score ≥ 70 → Elegível marketplace
  ↓
[Review Editorial (amostra)]
  └─ Aprovado → Listado marketplace
```

---

**Última Atualização**: 2025-12-26
