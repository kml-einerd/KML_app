# VERSIONING

## 1) Objetivo: Histórico de versões publicadas, changelog, rollback.
[fonte: 03 - todas as telas, flows e governança fechados.md → Publish & Versões → Versioning]

## 2) Usuário: Creator, **Contexto**: Quer ver histórico ou fazer rollback

## 3) Layout
```
**Versões Publicadas**
v1.2 (atual) - 20/12/2025
Changelog: "Corrigido beat 3"
[Rollback]

v1.1 - 15/12/2025
Changelog: "Adicionada aplicação"

v1.0 - 01/12/2025
Changelog: "Versão inicial"
```

## 4) CTA Primária: "Rollback" (com confirmação)
## 5) Estados: Success
## 6) Som: **STATUS**
## 7) Eventos: `versions_viewed`, `rollback_performed`
## 8) DoD: [ ] Histórico completo, [ ] Rollback funcional
## 9) Edições: **MicroSaaS**: Remove, **Standard/Full**: Mantém
## 10) Back: `GET /api/creator/infoapp/:id/versions`, `POST /api/creator/infoapp/:id/rollback`

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Versioning]
---
