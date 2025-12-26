# BADGES / GALERIA

## 1) Objetivo da Tela
Galeria visual de badges conquistados e bloqueados, reforçando identidade e conquistas.

[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Badges/Galeria]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver badges, **Frequência**: 1-2x por semana

## 3) Layout & Hierarquia
```
[Header: "Seus Badges"]

[Grid de Badges]
┌───┬───┬───┐
│ ✓ │ ✓ │🔒│ Conquistados: 5/12
├───┼───┼───┤
│ ✓ │🔒│🔒│
└───┴───┴───┘

[Badge Detail - Ao clicar]
"Mestre da Big Idea"
"Criou 10 Big Ideas validadas"
Conquistado em: 20/12/2025
```

## 4) Elementos & Componentes
- Grid de Badges (visual)
- Modal de detalhe (ao clicar)

## 5) Ação Primária
Tap badge → ver detalhe

## 6) Estados
Loading, Success

## 7) Conteúdo / Microcopy
- Badge names: Claros, não genéricos ("Mestre da Big Idea" > "Badge 1")
- Bloqueado: "Complete [tarefa] para desbloquear"

## 8) Som/Haptics
**STATUS**: `badge_view.mp3`, confetti se desbloqueio recente

## 9) Eventos
`badges_viewed`, `badge_detail_opened`

## 10) Definition of Done
- [ ] Grid visual funcional
- [ ] Modal de detalhe implementado
- [ ] Badges bloqueados indicam como desbloquear

## 11) Modo Studio / Edições
**MicroSaaS**: Remove (feature Standard+)
**Standard/Full**: Mantém

## 12) Mapeamento Back
`GET /api/learner/badges` → badges conquistados/bloqueados

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Badges/Galeria]

---
