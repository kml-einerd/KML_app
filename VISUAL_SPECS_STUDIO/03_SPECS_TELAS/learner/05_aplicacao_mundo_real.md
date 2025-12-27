# APLICAÇÃO (MUNDO REAL)

## 1) Objetivo da Tela
Forçar transferência para mundo real através de auto-declaração ou link (SEM upload de arquivos) com checklist e feedback coaching.
[fonte: Resposta Cliente #2 → REMOVER ou SIMPLIFICAR upload - evitar tarefas de upload]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Após módulo, tarefa real fora do app, **Frequência**: 1-3x por semana

## 3) Layout & Hierarquia
```
[Brief]
"Escreva 1 Big Idea do seu tema com promessa + mecanismo."

[Checklist Auto-Declarativo]
☐ Promessa clara
☐ Mecanismo crível
☐ Escrito e revisado

[ProofForm - SIMPLIFICADO]
[ Textarea: Descreva o que você fez ]
OU
[ Link URL (opcional): Cole o link se publicou online ]

[Botão: "Marcar como Feito"]

[Feedback - Após envio]
"Ótimo trabalho! Continue aplicando na prática."
[XP: +120]
```

## 4) Elementos & Componentes
- **Brief**: Contexto + objetivo (3 linhas)
- **Checklist**: 3-7 passos auto-declarativos (aluno marca ao fazer)
- **ProofForm SIMPLIFICADO**: Textarea (texto curto) OU Link URL (opcional)
- **FeedbackCard**: Encorajamento + próximo passo

**IMPORTANTE**: SEM upload de arquivos/screenshots. Validação baseada em honestidade + checklist.

[fonte: Resposta Cliente #2 → Aplicação SEM upload, apenas texto/link ou checklist auto-declarado]

## 5) Ação Primária
"Marcar como Feito" (após preencher checklist + texto/link opcional)

## 6) Estados
Loading, Empty, Error, Success (aplicação concluída)

## 7) Conteúdo / Microcopy
- Brief: Claro e direto
- Checklist: "Marque cada item conforme completar"
- Feedback: "Parabéns por aplicar no mundo real! Isso solidifica seu aprendizado."
- Placeholder Textarea: "Descreva brevemente o que você criou ou aprendeu ao fazer esta tarefa..."

## 8) Som/Haptics
**TENSION** (desafio com consequência leve): `ambient_tension.mp3`, `task_completed.mp3`

## 9) Eventos
`application_started`, `application_completed`, `checklist_checked`

## 10) Definition of Done
- [ ] Brief + Checklist claros
- [ ] ProofForm aceita texto curto OU link (SEM upload de arquivo)
- [ ] Feedback encorajador implementado
- [ ] XP alto (120-200) concedido
- [ ] Validação: Checklist completo + (texto OU link preenchido)

## 11) Modo Studio / Edições
**MicroSaaS**: Remove (feature Standard+)
**Standard/Full**: Mantém

## 12) Mapeamento Back
`POST /api/learner/application/submit` (payload: `checklist_items`, `description_text`, `url_link`)

**Validação**:
- Checklist completo (todos os items marcados)
- Texto (min 20 chars) OU link válido (formato URL)
- SEM upload de arquivos

**Anti-fraude**: Baseado em honestidade + padrões suspeitos (texto copiado, links spam)

[fonte: Resposta Cliente #2 → Cliente quer evitar tarefas de upload]

## 13) Rastreabilidade
[fonte: Resposta Cliente #2 → REMOVER ou SIMPLIFICAR upload]
[fonte: 04 - modo Studio.md → O que é "Aplicação"]
[fonte: 01 - economia.md → Cientista de Aprendizagem → Aplicação com prova]

---
