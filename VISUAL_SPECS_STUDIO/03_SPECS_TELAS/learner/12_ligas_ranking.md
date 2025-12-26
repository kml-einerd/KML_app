# LIGAS / RANKING (TEAM/PRIVATE)

## 1) Objetivo da Tela
Ranking governado (TEAM ou PRIVATE apenas, SEM ranking público) para competição saudável sem vergonha.

[fonte: 07 - alinhamento.md → Growth/Retention Designer → Ranking público removido]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver posição na liga, **Frequência**: 1-2x por semana

## 3) Layout & Hierarquia
```
[Header: "Sua Liga"]
Liga Bronze • Semana 12/2025

[Ranking - TEAM/PRIVATE]
1. João Silva - 450 XP
2. Você - 420 XP ←
3. Maria Costa - 380 XP
...
10. Pedro Lima - 200 XP

[Nota: "Ranking privado - apenas sua equipe"]
```

## 4) Elementos & Componentes
- Lista de ranking (top 10)
- Indicação clara "Você"
- Nota de privacidade

[fonte: 07 - alinhamento.md → Growth/Retention Designer → Ranking TEAM/PRIVATE]

## 5) Ação Primária
Nenhuma (visualização)

## 6) Estados
Loading, Empty ("Você ainda não está em uma liga"), Success

## 7) Conteúdo / Microcopy
- "Ranking privado - apenas sua equipe"
- "SEM exposição pública" (reforçar segurança)

## 8) Som/Haptics
**STATUS**: `ambient_status.mp3`

## 9) Eventos
`league_viewed`

## 10) Definition of Done
- [ ] Ranking exibe apenas TEAM/PRIVATE
- [ ] Nota de privacidade visível
- [ ] Posição do usuário destacada

## 11) Modo Studio / Edições
**MicroSaaS**: Remove (feature Full)
**Standard/Full**: Mantém (TEAM/PRIVATE apenas)

## 12) Mapeamento Back
`GET /api/learner/league` → ranking com filtro TEAM/PRIVATE

## 13) Rastreabilidade
[fonte: 07 - alinhamento.md → Growth/Retention Designer → Ranking público removido]
[fonte: 03 - todas as telas, flows e governança fechados.md → Social (governado)]

---
