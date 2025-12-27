# MODERAÇÃO / TRUST

## 1) Objetivo: Moderação de conteúdo e denúncias de usuários.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Moderação/Trust]

## 2) Usuário: Platform Admin, **Contexto**: Moderação e anti-abuso

## 3) Layout
```
**Fila de Moderação**

[Tabs]
[Denúncias] [Conteúdo Suspeito] [Bloqueios]

**Denúncias (5)**
• App "VSL Mastery" - Usuário reportou conteúdo ofensivo
  [Revisar] [Bloquear] [Ignorar]

**Conteúdo Suspeito (3)**
• Aplicação "Big Idea" - Texto suspeito de spam
  [Ver Detalhes] [Bloquear Usuário] [Ignorar]
```

## 4) CTA Primária: "Revisar"
## 5) Estados: Loading, Empty ("Nenhuma denúncia"), Success
## 6) Som: **SAFE**
## 7) Eventos: `moderation_queue_viewed`, `content_reviewed`, `user_blocked`
## 8) DoD: [ ] Fila de denúncias, [ ] Fila de conteúdo suspeito, [ ] Ações (bloquear/ignorar) funcionam
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/moderation/queue`, `POST /api/admin/moderation/action`

**IMPORTANTE**: Revisão de prova foi REMOVIDA (não há mais upload de arquivos em Aplicação).
Moderação agora foca em: denúncias de usuários + padrões suspeitos (spam, texto copiado, links maliciosos).

[fonte: Resposta Cliente #2 → REMOVER upload de prova]
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Trust & Safety → Moderação + Anti-fraude]
---
