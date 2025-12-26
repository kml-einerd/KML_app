# AJUDA / SUPORTE

## 1) Objetivo da Tela
FAQ + contato rápido para suporte.

## 2) Usuário & Contexto
**Usuário**: Learner com dúvida, **Contexto**: Precisa de ajuda, **Frequência**: Ocasional

## 3) Layout & Hierarquia
```
[Search Bar]
"Buscar ajuda..."

[FAQ Categorias]
📚 Como funciona?
🎯 Progresso e XP
🔊 Som e Acessibilidade

[Botão: "Falar com Suporte"]
```

## 4) Elementos & Componentes
- Search bar
- Accordion de FAQ
- Botão de contato

## 5) Ação Primária
"Falar com Suporte"

## 6) Estados
Success

## 7) Conteúdo / Microcopy
FAQ claro e direto

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`

## 9) Eventos
`help_viewed`, `faq_opened`, `support_contacted`

## 10) Definition of Done
- [ ] FAQ funcional
- [ ] Busca funciona
- [ ] Contato abre canal (email/chat)

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
FAQ estático ou `GET /api/help/faq`

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Feedback & Suporte]

---
