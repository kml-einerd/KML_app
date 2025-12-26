# ROLES & AUDIT

## 1) Objetivo: Gerenciar permissões (owner/editor/viewer) e trilha de auditoria.
[fonte: 03 - todas as telas, flows e governança fechados.md → Roles & Audit]

## 2) Usuário: Creator (owner), **Contexto**: Times multi-usuário

## 3) Layout
```
**Membros do Workspace**
• João Silva (Owner)
• Maria Costa (Editor)
• Pedro Lima (Viewer)
[Adicionar Membro]

**Trilha de Auditoria**
20/12 14:32 - João Silva publicou v1.1
19/12 10:15 - Maria Costa editou Aula 2
```

## 4) CTA Primária: "Adicionar Membro"
## 5) Estados: Success
## 6) Som: **SAFE**
## 7) Eventos: `member_added`, `audit_log_viewed`
## 8) DoD: [ ] Roles funcionam, [ ] Audit log completo
## 9) Edições: **MicroSaaS**: Remove (single user), **Full**: Mantém
## 10) Back: `GET /api/creator/workspace/:id/audit`

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Trust & Safety → Audit logs no Studio]
---
