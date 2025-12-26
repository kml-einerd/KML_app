# TENANTS / WORKSPACES (PLATFORM ADMIN)

## 1) Objetivo: Gestão global de workspaces (suporte, compliance).
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Tenants/Workspaces]

## 2) Usuário: Platform Admin (super-admin), **Contexto**: Suporte e compliance

## 3) Layout
```
[Search Workspace]

**Workspaces (120)**
• VSL Academy - 120 usuários - Plano: Full - Ativo
• Big Idea School - 45 usuários - Plano: Standard - Ativo
• Content Lab - 5 usuários - Plano: MicroSaaS - Suspenso

[Ações]
[Ver Detalhes] [Suspender] [Contatar]
```

## 4) CTA Primária: "Ver Detalhes"
## 5) Estados: Loading, Success
## 6) Som: **SAFE**
## 7) Eventos: `tenants_viewed`, `workspace_action`
## 8) DoD: [ ] Lista de workspaces, [ ] Search funciona, [ ] Ações admin funcionam
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/workspaces`

[fonte: 03 - todas as telas, flows e governança fechados.md → Platform Admin]
---
