# MODERAÇÃO / TRUST

## 1) Objetivo: Moderação de conteúdo, denúncias, fila de revisão de prova.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Moderação/Trust]

## 2) Usuário: Platform Admin, **Contexto**: Moderação e anti-abuso

## 3) Layout
```
**Fila de Moderação**

[Tabs]
[Denúncias] [Revisão de Prova] [Bloqueios]

**Denúncias (5)**
• App "VSL Mastery" - Usuário reportou conteúdo ofensivo
  [Revisar] [Bloquear] [Ignorar]

**Revisão de Prova (12)**
• Aplicação "Big Idea" - Screenshot enviado
  [Ver Prova] [Aprovar] [Reprovar]
```

## 4) CTA Primária: "Revisar"
## 5) Estados: Loading, Empty ("Nenhuma denúncia"), Success
## 6) Som: **SAFE**
## 7) Eventos: `moderation_queue_viewed`, `content_reviewed`, `proof_reviewed`
## 8) DoD: [ ] Fila de denúncias, [ ] Fila de prova, [ ] Ações (bloquear/aprovar) funcionam
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/moderation/queue`, `POST /api/admin/moderation/action`

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Trust & Safety → Moderação + Anti-fraude]
---
