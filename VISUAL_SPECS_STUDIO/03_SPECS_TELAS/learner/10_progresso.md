# PROGRESSO

## 1) Objetivo da Tela
Dashboard de progresso: Coins lifetime, Coins disponíveis, streak, histórico, badges.

[fonte: Resposta Cliente FINAL → Não tem níveis (apenas Coins e badges)]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver evolução, **Frequência**: 2-3x por semana

## 3) Layout & Hierarquia
```
[ProgressHeader]
📊 Seu Progresso

💰 Coins Lifetime: 1.850 (total acumulado)
💳 Coins Disponíveis: 245 (saldo para gastar)
🔥 Streak: 7 dias

[Gráfico Coins - Últimos 7 dias]
[Bar chart visual - Coins ganhos por dia]

[Histórico Recente]
• Aula "Big Idea" - 2h atrás (+25 Coins)
• Missão Rápida - 1 dia atrás (+10 Coins)
• Badge "Dedicado" - 2 dias atrás (+25 Coins)

[Link: "Ver Galeria de Badges"]
[Link: "Como Ganhar Mais Coins"]
```

## 4) Elementos & Componentes
- **ProgressHeader**: Exibe Coins Lifetime, Coins Disponíveis, Streak
- **Gráfico Coins**: Bar chart mostrando Coins ganhos nos últimos 7 dias
- **Lista de Histórico**: Últimas atividades que geraram Coins
- **Badge Count**: "5 / 12 badges conquistados"
- **CTAs**: "Ver Galeria de Badges", "Como Ganhar Mais Coins"

## 5) Ação Primária
"Ver Galeria de Badges"

## 6) Estados
Loading, Success

## 7) Conteúdo / Microcopy
- **Título**: "Seu Progresso" (simples e direto)
- **Coins Lifetime**: "Total acumulado: 1.850 Coins (nunca diminui)"
- **Coins Disponíveis**: "Disponível para gastar: 245 Coins"
- **Streak**: "7 dias consecutivos! Continue estudando para não perder."
- **Badges**: "Você conquistou 5 de 12 badges. Continue para desbloquear mais!"
- **Histórico vazio**: "Nenhuma atividade recente. Complete uma lição para ganhar Coins!"

## 8) Som/Haptics
**STATUS**: `ambient_status.mp3`

[fonte: 06_SISTEMA_SOM/mapa_som_por_estado.md → STATUS]

## 9) Eventos
`progress_viewed`

## 10) Definition of Done
- [ ] ProgressHeader exibe Coins Lifetime/Disponíveis/Streak
- [ ] Gráfico Coins funcional (bar chart últimos 7 dias)
- [ ] Histórico lista atividades recentes que geraram Coins
- [ ] Badge count exibido (X / Y badges)
- [ ] Link para Galeria de Badges funcional
- [ ] Link "Como Ganhar Mais Coins" abre modal explicativo

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`GET /api/learner/progress` → Coins lifetime, Coins disponíveis, streak, histórico
  Response:
  ```json
  {
    "coins_lifetime": 1850,
    "coins_balance": 245,
    "streak_days": 7,
    "badges_earned": 5,
    "badges_total": 12,
    "history": [
      { "activity": "Aula Big Idea", "coins": 25, "timestamp": "2025-12-27T14:30:00Z" },
      { "activity": "Missão Rápida", "coins": 10, "timestamp": "2025-12-26T10:15:00Z" }
    ],
    "coins_chart_7_days": [20, 35, 10, 45, 30, 25, 40]
  }
  ```

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Progresso → Progresso]

---
