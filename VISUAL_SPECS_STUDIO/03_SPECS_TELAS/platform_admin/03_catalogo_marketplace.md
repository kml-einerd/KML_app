# CATÁLOGO / MARKETPLACE

## 1) Objetivo: Listagem de InfoApps públicos, descoberta, featured/categorias.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Catálogo/Marketplace]

## 2) Usuário: Platform Admin, **Contexto**: Curadoria de marketplace

## 3) Layout
```
**Marketplace Público (45 apps)**

[Search] [Filtros: Nicho/Status]

**Apps Listados**
• VSL Mastery - 120 usuários - ⭐ 4.8
  [Featured] [Editar Categoria] [Remover]

• Big Idea Workshop - 45 usuários - ⭐ 4.5
  [Marcar Featured]

**Aguardando Aprovação (3)**
• Content Lab - Pendente review editorial
  [Revisar] [Aprovar] [Reprovar]
```

## 4) CTA Primária: "Revisar" (apps pendentes)
## 5) Estados: Loading, Success
## 6) Som: **SAFE**
## 7) Eventos: `marketplace_viewed`, `app_featured`, `app_approved`
## 8) DoD: [ ] Lista de apps, [ ] Featured funciona, [ ] Aprovação editorial funciona
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/marketplace`, `POST /api/admin/marketplace/feature`

[fonte: 07 - alinhamento.md → Head Product Design → Marketplace/Store público]
---
