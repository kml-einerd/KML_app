# ⚠️ PLATFORM ADMIN - DEPRECATED

**Data**: 2025-12-26
**Status**: DEPRECATED (Arquitetura antiga, não usar)
**Razão**: Reformulação completa da arquitetura baseada em respostas do cliente

---

## POR QUE FOI DEPRECATED?

**Cliente disse**: "acho que podemos matar a ideia de admin, a não ser que seja a nível de acesso"

### Arquitetura ANTIGA (❌ Incorreta):
```
Platform
├── Learner App (aluno)
├── Creator Studio (criador)
└── Platform Admin (operador) ← ESTE COMPONENTE
```

### Arquitetura NOVA (✅ Correta):
```
Platform (infraestrutura)
└── Modo Studio (criador cria InfoApps)
    └── InfoApp 1 (SaaS completo)
        ├── Learner App (aluno aprende)
        └── InfoApp Admin Panel (criador gerencia) ← SUBSTITUI Platform Admin
    └── InfoApp 2...
```

---

## O QUE ACONTECEU?

### Conceito Fundamental Mudou

**ANTES** (errado):
- Platform Admin era um "app separado" para operador da plataforma gerenciar:
  - Tenants/Workspaces
  - Moderação de conteúdo
  - Catálogo/Marketplace
  - Infraestrutura
  - Pagamentos
  - Auditoria

**DEPOIS** (correto):
O produto **NÃO é uma plataforma de cursos**. É um **Gerador de EdTech SaaS**.

- Cada InfoApp criado é um **SaaS completo e isolado**
- Cada InfoApp tem seu próprio **InfoApp Admin Panel**
- Platform Admin como "app separado" não faz sentido nessa arquitetura

---

## PARA ONDE FOI A FUNCIONALIDADE?

### Opção Escolhida: Platform Admin vira NÍVEL DE ACESSO

**Super Admin** (operador da plataforma):
- Entra no **Modo Studio** com permissões expandidas
- Vê todos os InfoApps criados (de todos os criadores)
- Pode moderar, suspender, auditar InfoApps
- Acessa analytics agregados de toda a plataforma

**Implementação**:
- Role: `super_admin` (vs `creator` normal)
- Interface: Modo Studio com tabs extras (Moderação, Analytics Global, Auditoria)

---

## ONDE ENCONTRAR A NOVA DOCUMENTAÇÃO?

### InfoApp Admin Panel (substitui Platform Admin)

**Localização das specs**: `/03_SPECS_TELAS/infoapp_admin/`

**Telas criadas**:
1. `01_dashboard.md` - Dashboard do InfoApp Admin
2. `02_gestao_conteudo.md` - CRUD de Lessons/Beats/Checkpoints
3. `03_upload_massa.md` - Import em massa (CSV/JSON/YAML)
4. `04_configurar_loja_recompensas.md` - Loja de Recompensas (MOVIDO daqui)
5. `05_usuarios_cohorts.md` - Gestão de alunos do InfoApp
6. `06_analytics.md` - Métricas do InfoApp
7. `07_configuracoes.md` - Branding, domínio, integrações

---

## SPECS ANTIGAS (Nesta Pasta) - NÃO USAR

**Arquivos deprecated**:
- ❌ `01_tenants_workspaces.md` → Não existe mais (InfoApps são criados no Modo Studio)
- ❌ `02_moderacao_trust.md` → Moderação vira nível de acesso no Modo Studio
- ❌ `03_catalogo_marketplace.md` → Não existe marketplace v1
- ❌ `04_infra_status.md` → Monitoramento é interno (não tem interface)
- ❌ `05_pagamentos.md` → Billing fica no Modo Studio (criador paga)
- ❌ `06_auditoria.md` → Auditoria vira nível de acesso no Modo Studio

**ATENÇÃO**: Não use estas specs para implementação. Elas refletem arquitetura antiga e incorreta.

---

## AÇÕES NECESSÁRIAS

### Para Desenvolvedores:
- ❌ NÃO implementar telas desta pasta
- ✅ Implementar telas de `/infoapp_admin/`
- ✅ Implementar sistema de roles (super_admin vs creator) no Modo Studio

### Para Designers:
- ❌ NÃO criar protótipos baseados nestas specs
- ✅ Criar protótipos de InfoApp Admin Panel

### Para Product:
- ✅ Validar nova arquitetura com cliente (ver `/16_CONFIRMACOES_CLIENTE/perguntas_criticas.md`)
- ✅ Atualizar roadmap

---

## DOCUMENTOS DE REFERÊNCIA

**Arquitetura nova**:
- `/13_ARQUITETURA_PRODUTO/arquitetura_conceitual.md` - Conceito Studio → InfoApp
- `/13_ARQUITETURA_PRODUTO/arquitetura_tecnica.md` - Multi-tenant strategy

**Perguntas para validação**:
- `/16_CONFIRMACOES_CLIENTE/perguntas_criticas.md` - Validar com cliente

**Specs corretas**:
- `/03_SPECS_TELAS/infoapp_admin/` - Nova interface admin

---

## HISTÓRICO

**2025-12-26**: Arquitetura reformulada baseada em respostas críticas do cliente
- Platform Admin deprecated
- InfoApp Admin Panel criado
- Conceito do produto redefinido (Gerador de EdTech SaaS)

---

**Status**: DEPRECATED - NÃO USAR
**Substituto**: InfoApp Admin Panel (`/03_SPECS_TELAS/infoapp_admin/`)
