# REVIEW SRS (REVISÃO ESPAÇADA)

## 1) Objetivo da Tela
Implementar revisão espaçada (Spaced Repetition System) para retenção estrutural de conhecimento via algoritmo SM-2.
[fonte: 03 - todas as telas, flows e governança fechados.md → Review SRS]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Agenda diária de revisão, **Frequência**: Diária (3-10 itens)

## 3) Layout & Hierarquia
```
[Header: "3 itens para revisar hoje"]
[SRS Card]
  Frente: "Big Idea = ?"
  [Botão: "Revelar"]

  Verso: "Promessa + Mecanismo"
  [Acertou?] [Quase] [Errou]
```

## 4) Elementos & Componentes
- **SRS Card**: frente/verso, botões de auto-avaliação
- **Intervalo SM-2**: Ajusta próxima revisão conforme resposta

## 5) Ação Primária
"Revelar" → Auto-avaliação (Acertou/Quase/Errou)

## 6) Estados
Loading, Empty ("Nenhum item para revisar hoje"), Success

## 7) Conteúdo / Microcopy
"Revisar agora ajuda a lembrar depois." (não infantilizar)

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`, `correct_soft.mp3` ao acertar

## 9) Eventos
`review_item_viewed`, `review_item_answered` (propriedades: `interval_next`, `ease_factor`)

## 10) Definition of Done
- [ ] SM-2 algoritmo implementado
- [ ] Cards com frente/verso
- [ ] Auto-avaliação funcional
- [ ] Agenda atualiza conforme respostas

## 11) Modo Studio / Edições
**MicroSaaS**: Remove (feature Full apenas)
**Standard/Full**: Mantém

## 12) Mapeamento Back
`GET /api/learner/srs/agenda`, `POST /api/learner/srs/answer` (atualiza intervalo)

## 13) Rastreabilidade
[fonte: 04 - modo Studio.md → srs/vocab.csv]
[fonte: 01 - economia.md → Cientista Comportamental → Revisão estrutural]

---
