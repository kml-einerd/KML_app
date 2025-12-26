# PROGRESSO

## 1) Objetivo da Tela
Dashboard de progresso: XP, nível, streak, histórico, badges em um só lugar.

[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Progresso]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver evolução, **Frequência**: 2-3x por semana

## 3) Layout & Hierarquia
```
[ProgressHeader]
Nível 5 • 476/500 XP
Streak: 🔥 7 dias

[Gráfico XP - Últimos 7 dias]
[Bar chart visual]

[Histórico Recente]
• Aula "Big Idea" - 2h atrás (+76 XP)
• Missão Rápida - 1 dia atrás (+15 XP)

[Link: "Ver Badges"]
```

## 4) Elementos & Componentes
- ProgressHeader (reutilizado da Home)
- Gráfico XP (bar chart últimos 7 dias)
- Lista de Histórico

## 5) Ação Primária
"Ver Badges"

## 6) Estados
Loading, Success

## 7) Conteúdo / Microcopy
"Seu progresso" (não "Parabéns por tudo!")

## 8) Som/Haptics
**STATUS**: `ambient_status.mp3`

[fonte: 06_SISTEMA_SOM/mapa_som_por_estado.md → STATUS]

## 9) Eventos
`progress_viewed`

## 10) Definition of Done
- [ ] ProgressHeader exibe XP/nível/streak
- [ ] Gráfico XP funcional
- [ ] Histórico lista atividades recentes

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`GET /api/learner/progress` → XP, streak, histórico

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Progresso]

---
