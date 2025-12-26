# AUDITORIA (PLATFORM ADMIN)

## 1) Objetivo: Logs globais, incidentes, segurança.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Auditoria]

## 2) Usuário: Platform Admin, **Contexto**: Segurança e compliance

## 3) Layout
```
[Search Logs] [Filtros: Data/User/Action]

**Logs de Auditoria**
20/12 14:32 - Admin João bloqueou app "VSL Mastery"
19/12 10:15 - Admin Maria aprovou app "Big Idea" para marketplace
15/12 08:00 - Sistema: Alerta de custo ElevenLabs

**Incidentes (1)**
🔴 15/12 - Tentativa de upload massivo de prova falsa
  Status: Resolvido
  [Ver Detalhes]
```

## 4) CTA Primária: "Ver Detalhes" (incidentes)
## 5) Estados: Loading, Success
## 6) Som: **SAFE**
## 7) Eventos: `audit_logs_viewed`, `incident_viewed`
## 8) DoD: [ ] Logs completos, [ ] Search/filtros funcionam, [ ] Incidentes rastreáveis
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/audit/logs`

[fonte: 03 - todas as telas, flows e governança fechados.md → Platform Admin → Auditoria]
---
