# TRILHA / MAPA

## 1) Objetivo da Tela
Mostrar progressão visual da trilha de aprendizagem (módulos, níveis, pré-requisitos) e "onde estou".

[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Mapa/Trilha]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver trilha completa, **Frequência**: 2-3x por semana

## 3) Layout & Hierarquia
```
[Header: "Sua Trilha"]

[Mapa Visual - Vertical Scroll]
┌─────────┐
│ Módulo 1│ ✓ Concluído
└─────────┘
    ↓
┌─────────┐
│ Módulo 2│ → Em Progresso (3/5 aulas)
└─────────┘
    ↓
┌─────────┐
│ Módulo 3│ 🔒 Bloqueado (pré-requisito: Módulo 2)
└─────────┘

[CTA: "Continuar Módulo 2"]
```

## 4) Elementos & Componentes
- **Módulos**: Cards com estado (concluído/em progresso/bloqueado)
- **Conectores**: Linhas verticais mostrando sequência
- **Pré-requisitos**: Indicação visual de bloqueio

## 5) Ação Primária
"Continuar [Módulo atual]"

## 6) Estados
Loading, Success

## 7) Conteúdo / Microcopy
- Estado: "Concluído", "Em Progresso (3/5)", "Bloqueado"
- Bloqueio: "Complete [Módulo anterior] para desbloquear"

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`, `tap_soft.mp3`

## 9) Eventos
`trail_viewed`, `module_tapped`

## 10) Definition of Done
- [ ] Mapa visual com módulos
- [ ] Estados claros (concluído/progresso/bloqueado)
- [ ] Pré-requisitos exibidos
- [ ] CTA leva ao módulo correto

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`GET /api/learner/trail/:infoapp_id` → retorna módulos com progressão

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Mapa/Trilha]

---
