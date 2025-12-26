# APLICAÇÃO (MUNDO REAL)

## 1) Objetivo da Tela
Forçar transferência para mundo real através de prova objetiva (upload/texto/link) com rubrica e feedback coaching.
[fonte: 04 - modo Studio.md → O que é "Aplicação"]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Após módulo, tarefa real fora do app, **Frequência**: 1-3x por semana

## 3) Layout & Hierarquia
```
[Brief]
"Escreva 1 Big Idea do seu tema com promessa + mecanismo."

[Checklist]
☑ Promessa clara
☑ Mecanismo crível
☑ Escrito

[ProofForm]
[ Texto / Upload / Link ]
[Botão: "Enviar Prova"]

[Rubrica - Após envio]
✓ Promessa: OK
⚠ Mecanismo: Fraco
Feedback: "Seu mecanismo ainda é genérico..."

[XP: +120]
```

## 4) Elementos & Componentes
- **Brief**: Contexto + objetivo (3 linhas)
- **Checklist**: 3-7 passos
- **ProofForm**: Textarea, FileUpload, ou LinkInput
- **Rubrica**: Critérios objetivos (0/1 ou 0-2)
- **FeedbackCard**: Coaching + insight

[fonte: 04 - modo Studio.md → Aplicação na prática → tabela]

## 5) Ação Primária
"Enviar Prova" (após preencher)

## 6) Estados
Loading, Empty, Error, Success (prova aceita)

## 7) Conteúdo / Microcopy
- Brief: Claro e direto
- Checklist: Reduz incerteza
- Feedback: Coaching (não punição)

## 8) Som/Haptics
**TENSION** (desafio com consequência leve): `ambient_tension.mp3`, `upload_success.mp3`

## 9) Eventos
`application_started`, `proof_submitted`, `rubric_evaluated`

## 10) Definition of Done
- [ ] Brief + Checklist claros
- [ ] ProofForm aceita texto/upload/link
- [ ] Rubrica valida critérios
- [ ] Feedback coaching implementado
- [ ] XP alto (120-200) concedido

## 11) Modo Studio / Edições
**MicroSaaS**: Remove (feature Standard+)
**Standard/Full**: Mantém

## 12) Mapeamento Back
`POST /api/learner/application/submit` (payload: `proof_type`, `proof_content`)
Anti-fraud: validação de uploads (não screenshots obviamente falsos)
[fonte: 07 - alinhamento.md → Trust & Safety → Anti-fraude em uploads]

## 13) Rastreabilidade
[fonte: 04 - modo Studio.md → O que é "Aplicação"]
[fonte: 01 - economia.md → Cientista de Aprendizagem → Aplicação com prova]

---
