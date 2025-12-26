# ONBOARDING: OBJETIVO

## 1) Objetivo da Tela
Capturar objetivo principal do aluno para personalizar experiência (primeira tela de 4 no onboarding).

[fonte: 03 - todas as telas, flows e governança fechados.md → Onboarding (4 telas)]

## 2) Usuário & Contexto
**Usuário**: Novo aluno (pós-signup), **Contexto**: Primeira configuração, **Frequência**: 1x

## 3) Layout & Hierarquia
```
"Por que você está aqui?"

[Opções - Radio Select]
○ Aprender algo novo
○ Melhorar habilidade existente
○ Preparar para projeto
○ Outro

[Botão: "Próximo"]
[Progresso: 1/4]
```

## 4) Elementos & Componentes
- Radio buttons (único selecionável)
- Botão "Próximo" (desabilitado até selecionar)

## 5) Ação Primária
"Próximo"

## 6) Estados
Success (seleção feita)

## 7) Conteúdo / Microcopy
"Por que você está aqui?" (direto, sem floreio)

## 8) Som/Haptics
**SAFE**: `select_soft.mp3`

## 9) Eventos
`onboarding_step_completed` (step: 1, choice: `objective`)

## 10) Definition of Done
- [ ] Radio select funcional
- [ ] "Próximo" desabilitado até selecionar
- [ ] Redireciona à próxima tela (Nível)

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`POST /api/learner/onboarding/step` (payload: `step`, `data`)

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Onboarding → Objetivo]

---
