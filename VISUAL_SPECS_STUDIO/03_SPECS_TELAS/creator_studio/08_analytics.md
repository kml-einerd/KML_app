# ANALYTICS

## 1) Objetivo da Tela
Dashboard de analytics: funil de conclusão, drop-off por beat, cohorts, heatmaps.

[fonte: 03 - todas as telas, flows e governança fechados.md → Analytics → Funil por beat, drop-off, cohorts, heatmaps]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer otimizar retenção, **Frequência**: 2-3x por semana

## 3) Layout & Hierarquia
```
[Header: "Analytics - VSL Mastery"]

[Métricas-Chave]
120 usuários ativos • 75% conclusão • 5.2 D1 retorno

[Tab Bar]
[Funil] [Drop-off por Beat] [Cohorts] [Heatmaps]

[Tab Ativo: Drop-off por Beat]
Aula 1: "Big Idea"
Beat 1: 100% (120/120)
Beat 2: 95% (114/120)
Beat 3: 80% (96/120) ⚠️ Drop aqui
Beat 4: 78% (94/120)
...

[Insights]
"Beat 3 tem maior drop. Considere revisar checkpoint."

[Botão: "Exportar CSV"]
```

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Data/Analytics Lead → Analytics por beat + drop-off]

## 4) Elementos & Componentes
- **Métricas-Chave**: Cards com números principais
- **Tab Bar**: Funil, Drop-off, Cohorts, Heatmaps
- **Charts**: Gráficos de barra, linha
- **Insights**: Alertas automáticos (ex: "Beat com maior drop")

## 5) Ação Primária
"Exportar CSV" (dados para análise externa)

## 6) Estados
- **Loading**: Carregando analytics
- **Empty**: "Sem dados ainda (mínimo 10 usuários)"
- **Success**: Charts exibidos

## 7) Conteúdo / Microcopy
- "Drop-off por Beat"
- "Cohorts (por semana)"
- Insights: Automáticos e acionáveis

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`

## 9) Eventos
`analytics_viewed`, `tab_changed`, `csv_exported`

## 10) Definition of Done
- [ ] Métricas-chave exibidas
- [ ] Tabs funcionam (Funil/Drop-off/Cohorts/Heatmaps)
- [ ] Drop-off por beat funcional
- [ ] Exportar CSV funciona

## 11) Modo Studio / Edições
- **MicroSaaS**: Remove (feature Full)
- **Standard**: Analytics básico (funil + drop-off)
- **Full**: Analytics completo (+ cohorts + heatmaps)

## 12) Mapeamento Back
`GET /api/creator/infoapp/:id/analytics` → métricas agregadas

Atalho: Biblioteca de charts (Recharts ou Chart.js)

## 13) Rastreabilidade
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Data/Analytics Lead → Dashboards-alvo]
[fonte: 03 - todas as telas, flows e governança fechados.md → Analytics]

---
