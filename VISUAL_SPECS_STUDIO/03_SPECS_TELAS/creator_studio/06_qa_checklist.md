# QA CHECKLIST

## 1) Objetivo da Tela
Validar app antes de publicar via quality gates automáticos. Bloqueia publicação se gates falharem.

[fonte: 05 - sistema completo de EdTech.md → Quality Gates automáticos]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Antes de publish, **Frequência**: 1x antes de cada publish

## 3) Layout & Hierarquia
```
[Header: "QA Checklist"]

[Gates - Status]
✓ Gate 1: Atividade (checkpoint por beat) - OK
✓ Gate 2: Transferência (aplicação por módulo) - OK
✓ Gate 3: Progressão (pré-requisitos claros) - OK
✓ Gate 4: Feedback (checkpoints têm feedback) - OK
⚠️ Gate 5: Tempo/Escopo (beats 45-90s) - WARNING: Beat 7 tem 95s
❌ Gate 6: Clareza (objetivo da aula) - ERRO: Aula 3 sem objetivo

[Banner - Se erro]
🚫 Publicação Bloqueada
"Corrija os erros antes de publicar."

[Botões]
[Corrigir Agora] [Fechar]
```

[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 4. PUBLISH GATES]

## 4) Elementos & Componentes
- **Gate Items**: Lista com status (OK/WARNING/ERROR)
- **Banner de Bloqueio**: Vermelho, visível se qualquer ERROR
- **Links**: "Corrigir Agora" leva à tela/item específico

## 5) Ação Primária
Se OK: "Publicar" (redireciona a Publish)
Se ERROR: "Corrigir Agora"

## 6) Estados
- **Loading**: Validando gates
- **Success**: Todos gates OK
- **Warning**: Alguns warnings (pode publicar)
- **Error**: Algum gate failed (bloqueia)

## 7) Conteúdo / Microcopy
- Gate 1: "Atividade (checkpoint por beat)"
- Gate 2: "Transferência (aplicação por módulo)"
- Erro: "Aula sem checkpoint bloqueia publicação"

**Princípio**: Governança automática, não manual.

[fonte: 01 - economia.md → Head of Product → Quality Gates bloqueiam publish]

## 8) Som/Haptics
**SAFE**: `qa_complete.mp3` (se OK), `error_soft.mp3` (se erro)

## 9) Eventos
`qa_started`, `qa_completed` (propriedades: `gates_passed`, `gates_failed`, `gates_warnings`)

## 10) Definition of Done
- [ ] 6 gates validados automaticamente
- [ ] Erros bloqueiam publicação (banner vermelho)
- [ ] Warnings permitem publicação mas alertam
- [ ] "Corrigir Agora" leva à tela/item específico

## 11) Modo Studio / Edições
Todos mantêm (governança obrigatória)

## 12) Mapeamento Back
`POST /api/creator/infoapp/:id/validate` → retorna gates com status

Validações server-side:
- Gate 1: Checar beats sem checkpoint
- Gate 2: Checar módulos sem aplicação
- Gate 3: Checar trilha sem pré-requisitos
- Gate 4: Checar checkpoints sem feedback
- Gate 5: Checar beats fora de 45-90s
- Gate 6: Checar aulas sem objetivo

[fonte: 05 - sistema completo de EdTech.md → Gates Obrigatórios]

## 13) Rastreabilidade
[fonte: 05 - sistema completo de EdTech.md → Quality Gates automáticos]
[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 4.1 Gates Obrigatórios]

---
