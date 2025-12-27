# MAPA DE NAVEGAÇÃO OFICIAL

**Navegação Oficial dos 3 Apps: Learner / Creator Studio / Platform Admin**

Este documento define a navegação oficial, jornadas, pontos de decisão críticos, checkpoints e publish gates para os três aplicativos.

[fonte: 03 - todas as telas, flows e governança fechados.md → Navegação oficial]

---

## PRINCÍPIO DE NAVEGAÇÃO

**Regra de ouro**: Menos caminhos = mais conclusão.

- Cada app tem navegação consistente e previsível
- 1 CTA dominante por tela (evita paralisia de decisão)
- "Continuar" sempre visível no Learner
- Jornadas lineares no Creator Studio (Create → Build → QA → Publish)

[fonte: 03 - todas as telas, flows e governança fechados.md → 3) Navegação oficial]

---

## 1. LEARNER APP (ALUNO)

### 1.1 Padrão de Navegação

[fonte: 03 - todas as telas, flows e governança fechados.md → 3.1 Navegação do Aluno]

| Elemento | Configuração | Regra |
|----------|--------------|-------|
| **Bottom Tabs (5)** | Home • Trilha • Review • Progresso • Perfil | 1 CTA dominante por tela |
| **Botão "Continuar"** | Sempre visível no topo da Home | Reduz fricção; leva à última atividade |
| **Player** | Modal/stack interno | Checkpoint dentro do Player (não tela separada) |
| **Checkpoint** | Componente do Player | Bloqueia avanço até responder |

### 1.2 Seções e Telas do Learner

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.1 Learner App]

#### ENTRADA
- Login
- Signup
- Recuperar Senha
- Consentimentos (LGPD/privacidade)

#### ONBOARDING (4 telas)
1. Objetivo (por que está aqui?)
2. Nível (iniciante/intermediário/avançado)
3. Rotina (quando estudará?)
4. Preferências (notificações opt-in, som, motion)

**Princípio**: "Safe by default" — notificações são opt-in, não opt-out.

#### CORE DIÁRIO
- **Home (Missão do Dia)**
  - 1 CTA dominante: "Começar Missão" ou "Continuar"
  - ProgressHeader com streak
  - Próximo passo sugerido

#### CONTEÚDO (Player Unificado)
[fonte: 03 - todas as telas, flows e governança fechados.md → Player unificado com modes]

- **Player** (único para todos os formatos)
  - Modo: Aula Interativa
  - Modo: Story Interativa
  - Modo: Missão Rápida
  - CheckpointModule (componente interno)
  - Progresso de beat
  - Toggle de som/legendas

- **Review SRS** (Revisão Espaçada)
  - Agenda SM-2
  - Cards de revisão

- **Aplicação** (Mundo Real)
  - Brief + Checklist
  - ProofForm (upload/texto/link)
  - Rubrica + Feedback

#### PROGRESSO
- Mapa/Trilha (onde estou?)
- Progresso (XP, nível, streak)
- Badges/Galeria
- Histórico de conclusões

#### ECONOMIA
- Marketplace/Loja de Recompensas (gastar moedas)
- Carteira/Saldo XP e Moedas
- Histórico de Resgates

#### SOCIAL (GOVERNADO)
[fonte: 07 - alinhamento.md → Growth/Retention Designer → Ranking público removido]

- Ligas/Ranking (TEAM/PRIVATE apenas)
- Equipe/Grupo
- **SEM ranking público** (risco de vergonha/toxicidade)

#### FEEDBACK & SUPORTE
- Feedback Rápido (in-app)
- Reportar Problema
- Ajuda/FAQ
- Contato

#### CONTA
- Perfil
- Configurações (som/haptics/motion/notificações)
- Privacidade
- Sair

### 1.3 Jornada Principal do Aluno

```
[Signup/Login]
  ↓
[Onboarding: 4 telas]
  ↓
[Home: Missão do Dia]
  ↓
[Player: Aula Interativa com checkpoints]
  ↓ (checkpoint bloqueia avanço)
[Player: Próximo Beat]
  ↓ (8 beats)
[Conclusão: XP Breakdown]
  ↓
[Home: Próxima Missão ou Review SRS]
```

**Pontos de Decisão Críticos**:
- **Checkpoint**: Bloqueia avanço até responder
- **Aplicação**: Exige prova para conclusão
- **Review SRS**: Agenda obrigatória para retenção

### 1.4 Navegação Bottom Tabs (Detalhada)

| Tab | Tela Principal | Subtelas | CTA Primário |
|-----|----------------|----------|--------------|
| **Home** | Home (Missão do Dia) | - | "Começar" ou "Continuar" |
| **Trilha** | Mapa da Trilha | Módulos, Níveis | "Ir para próxima aula" |
| **Review** | Review SRS | Agenda de revisão | "Revisar agora" |
| **Progresso** | Dashboard de Progresso | Badges, Histórico, Ligas | "Ver badges" |
| **Perfil** | Perfil | Configurações, Ajuda | "Editar perfil" |

---

## 2. CREATOR STUDIO (CRIADOR)

### 2.1 Padrão de Navegação

[fonte: 03 - todas as telas, flows e governança fechados.md → 3.2 Navegação do Criador]

| Elemento | Configuração | Regra |
|----------|--------------|-------|
| **Sidebar (8-10 itens)** | Apps • Build • Preview • QA • Publish • Analytics • Users • Rewards • Settings • Billing | Navegação vertical sempre visível |
| **Barra Superior** | Workspace Switch + Search | Troca rápida de workspace |
| **Jornada Linear** | Create → Build → QA → Publish | Sempre nesta ordem |

### 2.2 Seções e Telas do Creator Studio

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.2 Creator Studio]

#### WORKSPACE
- Login Studio
- Selecionar Workspace
- Criar Workspace

#### DASHBOARD
- Visão Geral (métricas)
- Lista de Apps (draft/published)
- Atalhos (Criar/Importar/Publish)

#### CRIAR INFOAPP
- **Wizard (4-5 passos)**:
  1. Nome + Nicho
  2. Idioma + Público
  3. Branding (cor/logo)
  4. Objetivos de Aprendizagem
  5. Review + Criar

**Princípio**: Criador não "monta app", escolhe formato oficial.

#### BUILD (Linha de Montagem)

[fonte: 04 - modo Studio.md → O que é "Aba do Creator"]

- **Import Pack** (upload .zip)
  - Validação de estrutura
  - Preview de conteúdo
  - Erros/Warnings

- **Build por Formato**:
  - Build Aula Interativa (beats + checkpoints)
  - Build Story Interativa
  - Build Missão Rápida
  - Build Review SRS (CSV)
  - Build Aplicação (prova + rubrica)

#### PREVIEW
- **Preview Emulator** (simula telas do aluno)
  - Preview por perfil (SAFE/TENSION/STATUS)
  - Preview por dispositivo (mobile/tablet)

#### QA & GOVERNANÇA

[fonte: 05 - sistema completo de EdTech.md → Quality Gates]

- **QA Checklist**
  - Gate 1: Atividade (checkpoint por beat)
  - Gate 2: Transferência (aplicação por módulo)
  - Gate 3: Progressão (pré-requisitos claros)
  - Gate 4: Feedback (todo checkpoint tem feedback)
  - Gate 5: Tempo/Escopo (beats 45-90s)
  - Gate 6: Clareza (objetivo da aula)

- **Estado "Bloqueado"**:
  - Banner vermelho: "Corrigir agora"
  - Lista de bloqueios
  - Não pode publicar até resolver

#### PUBLISH & VERSÕES
- **Publish**
  - Review final
  - Termos de Uso e Políticas
  - Botão "Publicar"

- **Versioning**
  - Changelog
  - Rollback
  - Histórico de versões

#### CONTEÚDO & BIBLIOTECA
- Content Library (reutilizar blocos)
- Assets Library (imagens/svg/anim)
- Templates (import packs prontos)

#### ECONOMIA & RECOMPENSAS
- XP Rules (presets + avançado)
- Badges (criar/editar)
- Loja de Recompensas (ativar/configurar produtos custom)

#### ANALYTICS

[fonte: 09 - visão final e condições.md → Data/Analytics Lead]

- Funil de Conclusão
- Drop-off por Beat
- Cohorts
- Heatmaps

#### USUÁRIOS & COMUNIDADE
- Lista de Usuários
- Cohorts
- Feedback Recebido
- Suporte

#### CONFIGURAÇÕES
- Domínio/SEO/Store
- Integrações (TTS: ElevenLabs)
- Notificações Push
- Branding

#### BILLING
- Plano Atual
- Limites/Quotas (TTS, storage, usuários)
- Faturas
- Upgrade

#### ROLES & AUDIT
- Permissões (owner/editor/viewer)
- Logs de Auditoria
- Trilha de Ações

### 2.3 Jornada Principal do Criador

```
[Login Studio]
  ↓
[Selecionar Workspace]
  ↓
[Dashboard]
  ↓
[Criar InfoApp: Wizard]
  ↓
[Import Pack ou Build Manual]
  ↓
[Preview Emulator]
  ↓
[QA Checklist]
  ↓ (se gates OK)
[Publish]
  ↓
[Analytics: Acompanhar Performance]
```

**Pontos de Decisão Críticos**:
- **QA Gates**: Bloqueia publicação se não cumprir critérios
- **Versioning**: Permite rollback se publicar versão ruim
- **Preview**: Obrigatório antes de publish (vê como aluno vê)

### 2.4 Navegação Sidebar (Detalhada)

| Item | Tela Principal | Subtelas | CTA Primário |
|------|----------------|----------|--------------|
| **Apps** | Lista de InfoApps | Draft, Published | "Criar novo app" |
| **Build** | Import Pack | Build por formato | "Importar pack" |
| **Preview** | Preview Emulator | Por perfil, dispositivo | "Preview como aluno" |
| **QA** | QA Checklist | Bloqueios, Warnings | "Corrigir agora" |
| **Publish** | Publish | Versioning | "Publicar versão" |
| **Analytics** | Dashboard Analytics | Funil, Drop-off, Cohorts | "Ver drop-off" |
| **Users** | Lista de Usuários | Cohorts, Feedback | "Ver cohorts" |
| **Rewards** | XP/Badges/Loja | Economia | "Editar XP" |
| **Settings** | Configurações | Domínio, TTS, Branding | "Salvar" |
| **Billing** | Plano & Faturas | Quotas | "Upgrade" |

---

## 3. PLATFORM ADMIN (OPERADOR DA PLATAFORMA)

### 3.1 Padrão de Navegação

| Elemento | Configuração | Regra |
|----------|--------------|-------|
| **Sidebar (6-8 itens)** | Tenants • Moderação • Catálogo • Infra • Pagamentos • Auditoria | Admin-only |
| **Permissões** | Somente super-admin | Logs completos |

### 3.2 Seções e Telas do Platform Admin

[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin]

#### TENANTS/WORKSPACES
- Gestão Global de Workspaces
- Suporte
- Compliance

#### MODERAÇÃO/TRUST

[fonte: 07 - alinhamento.md → Trust & Safety]

- Denúncias (reportes de usuários)
- Fila de Revisão de Conteúdo
- Fila de Revisão de Prova (aplicações)
- Bloqueios
- Políticas de Conteúdo

#### LOJA DE RECOMPENSAS (CONFIGURAÇÃO)
- Catálogo Global de Produtos/Benefícios
- Criar/Editar Produtos
- Categorias (Descontos, Premium, Gift Cards, Físicos)
- Ver Resgates (histórico de compras)

#### INFRA/STATUS
- Saúde do Sistema
- Alertas de Custo (ElevenLabs, storage)
- Performance

#### PAGAMENTOS
- Planos (MicroSaaS/Standard/Full)
- Cobrança
- Chargebacks

#### AUDITORIA
- Logs Globais
- Incidentes
- Segurança

### 3.3 Jornada Principal do Admin

```
[Login Admin]
  ↓
[Dashboard: Visão Global]
  ↓
[Moderação: Revisar Denúncias]
  ↓
[Loja: Configurar Produtos de Recompensas]
  ↓
[Infra: Monitorar Custos/Saúde]
```

**Pontos de Decisão Críticos**:
- **Moderação**: Bloquear/aprovar conteúdo denunciado
- **Loja**: Configurar produtos/benefícios que alunos podem resgatar
- **Infra**: Alertas de custo exigem ação imediata

[fonte: Resposta Cliente #3 → Marketplace é LOJA DE RECOMPENSAS, não catálogo de apps]

---

## 4. PUBLISH GATES (BLOQUEIOS DE PUBLICAÇÃO)

[fonte: 05 - sistema completo de EdTech.md → Quality Gates automáticos]

### 4.1 Gates Obrigatórios (Bloqueiam Publicação)

| Gate | Regra | Se Falhar | Onde Verifica |
|------|-------|-----------|---------------|
| **Gate 1: Atividade** | Aula tem checkpoint em todo beat | Não publica | Creator Studio: QA Checklist |
| **Gate 2: Transferência** | Cada módulo tem pelo menos 1 aplicação | Não publica (ou marca "beta") | Creator Studio: QA Checklist |
| **Gate 3: Progressão** | Trilha tem pré-requisitos claros/ordem | Não publica | Creator Studio: QA Checklist |
| **Gate 4: Feedback** | Todo checkpoint tem feedback (não só certo/errado) | Não publica | Creator Studio: QA Checklist |
| **Gate 5: Tempo/Escopo** | Beats 45-90s (tolerância) | Alerta/ajuste automático | Import Pack: Validação |
| **Gate 6: Clareza** | Objetivo da aula + "o que você saberá fazer" | Não publica | Creator Studio: QA Checklist |

### 4.2 Score de Qualidade (Para Publicação)

[fonte: 05 - sistema completo de EdTech.md → Score de Qualidade]

| Dimensão | Como Medir | Alvo |
|----------|------------|------|
| **Atividade** | % beats com checkpoint | 100% |
| **Transferência** | Apps com prova/módulo | ≥1 |
| **Profundidade** | Nº de simulações/aplicações por trilha | Mínimo por nível |
| **Retenção** | SRS presente quando faz sentido | Sim/Não |
| **Clareza** | Objetivo + resumo + ação aplicada | 100% |
| **Engajamento Real** | Completion rate + retorno D1/D7 | Thresholds |

**Apps abaixo do score**:
- Bloqueados de publicação até atingir threshold
- Ou aparecem com selo "beta" (criador escolhe publicar mesmo com score baixo)

**NOTA**: Discovery/marketplace de apps públicos vai para v1.5. v1 tem apenas QA gates para publicação.

[fonte: Resposta Cliente #10 → SEO/Discovery v1.5]

---

## 5. PONTOS DE DECISÃO CRÍTICOS (RESUMO)

### Learner App
1. **Checkpoint no Player**: Bloqueia avanço até responder
2. **Aplicação**: Exige prova para conclusão
3. **Review SRS**: Agenda obrigatória

### Creator Studio
1. **QA Gates**: Bloqueia publicação se não cumprir
2. **Preview Obrigatório**: Simula experiência do aluno antes de publicar
3. **Versioning**: Permite rollback

### Platform Admin
1. **Moderação**: Aprovar/bloquear conteúdo denunciado
2. **Loja de Recompensas**: Configurar produtos/benefícios globais
3. **Alertas de Custo**: Ação imediata necessária (TTS, storage)

---

## 6. NAVEGAÇÃO TRANSVERSAL (ENTRE APPS)

### 6.1 Workspace Switch (Creator Studio)

- Criador pode ter múltiplos workspaces
- Troca rápida na barra superior
- Cada workspace = 1 marca/produto

### 6.2 Modo Demo/Preview Público

- Criador pode compartilhar link de preview do seu app
- Usuário vê telas do app sem logar
- CTA: "Começar agora" → Signup

**NOTA**: Discovery/marketplace público de apps vai para v1.5. v1 tem apenas preview direto via link.

[fonte: Resposta Cliente #10 → SEO/Discovery v1.5, cada produto tem funil próprio]

### 6.3 Search Global

[fonte: 07 - alinhamento.md → UX Architect → Search global]

- **Learner**: Busca aulas, trilhas, badges
- **Creator Studio**: Busca apps, conteúdo, templates
- Filtros por tipo, status, data

---

## RESUMO

### Learner
- **Navegação**: Bottom tabs (5)
- **Jornada**: Linear (Home → Player → Conclusão)
- **Gates**: Checkpoint bloqueia avanço; Aplicação exige prova

### Creator Studio
- **Navegação**: Sidebar (8-10)
- **Jornada**: Linear (Create → Build → QA → Publish)
- **Gates**: QA Checklist bloqueia publicação

### Platform Admin
- **Navegação**: Sidebar (6-8)
- **Jornada**: Moderação → Loja de Recompensas → Infra
- **Gates**: Moderação de conteúdo + configuração de produtos da loja

---

**Última Atualização**: 2025-12-26
