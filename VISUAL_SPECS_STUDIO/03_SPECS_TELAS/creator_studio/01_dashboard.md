# DASHBOARD (CREATOR STUDIO)

## 1) Objetivo da Tela
Hub central do criador: visão geral de apps, métricas-chave e atalhos para criar/importar/publicar.

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.2 Creator Studio → Dashboard]

## 2) Usuário & Contexto
**Usuário**: Creator (criador de conteúdo), **Contexto**: Primeira tela após login no Studio, **Frequência**: Diária

## 3) Layout & Hierarquia
```
┌─────────────────────────────────────┐
│ [TopBar]                            │
│ [Workspace Switch ▼] [Search 🔍]    │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Sidebar - Sempre Visível]          │
│ Apps                                │
│ Build                               │
│ Preview                             │
│ QA                                  │
│ Publish                             │
│ Analytics                           │
│ Users                               │
│ Rewards                             │
│ Settings                            │
│ Billing                             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ [Main Content]                      │
│                                     │
│ Seus InfoApps (3)                   │
│                                     │
│ [Card: "VSL Mastery"]               │
│ Status: Published • 120 usuários    │
│ [Ver Analytics] [Editar]            │
│                                     │
│ [Card: "Big Idea Workshop"]         │
│ Status: Draft                       │
│ [Continuar Build] [Preview]         │
│                                     │
│ [Botão Primário: "+ Criar Novo App"]│
└─────────────────────────────────────┘
```

[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 2.1 Padrão de Navegação Creator Studio]

## 4) Elementos & Componentes
- **TopBar**: Workspace Switch (dropdown), Search (global)
- **Sidebar**: Navegação vertical (8-10 itens)
- **InfoApp Cards**: título, status, métricas, CTAs
- **Botão Primário**: "Criar Novo App"

[fonte: 03 - todas as telas, flows e governança fechados.md → 3.2 Navegação do Criador]

## 5) Ação Primária
"+ Criar Novo App" (se não há apps) ou "Continuar Build" (se há draft)

## 6) Estados
- **Loading**: Skeleton de cards
- **Empty**: "Crie seu primeiro InfoApp" + ilustração
- **Success**: Lista de apps exibida

## 7) Conteúdo / Microcopy
- "Seus InfoApps"
- Status: "Published", "Draft", "QA Failed"
- Empty: "Comece criando seu primeiro InfoApp"

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3` (ambiente de trabalho)

## 9) Eventos
`dashboard_viewed`, `app_card_clicked`, `create_new_clicked`

## 10) Definition of Done
- [ ] Sidebar sempre visível
- [ ] Workspace Switch funcional
- [ ] Cards exibem apps com status correto
- [ ] Search global funciona
- [ ] 3 estados (Loading/Empty/Success)

## 11) Modo Studio / Edições
- **MicroSaaS**: Limita 1 app
- **Standard**: Até 5 apps
- **Full**: Ilimitado

[fonte: 05_VARIANTES_MODO_STUDIO/matriz_edicoes.md]

## 12) Mapeamento Back
`GET /api/creator/workspace/:id/apps` → lista de apps com métricas

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Creator Studio → Dashboard]
[fonte: 02_MAPA_NAVEGACAO_OFICIAL.md → 2.2 Seções do Creator Studio]

---
