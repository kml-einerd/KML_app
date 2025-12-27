# Arquitetura Conceitual do Produto

**Versão**: 2.0
**Data**: 2025-12-26
**Status**: OFICIAL
**Mudança crítica**: Reformulação completa baseada em respostas do cliente

---

## 1. CONCEITO FUNDAMENTAL DO PRODUTO

### O que NÃO somos
- ❌ Plataforma de cursos online tradicional
- ❌ LMS onde criadores publicam conteúdo
- ❌ Marketplace de educação

### O que SOMOS
✅ **Gerador de EdTech SaaS** (InfoApp Creator)

O produto permite que **qualquer pessoa crie um SaaS de educação completo do zero**, agnóstico de tema ou conteúdo.

**Valor único**: Transformar conteúdo (ebook, PDF, vídeo, etc.) em um SaaS de educação interativo e completo, com gamificação, loja de recompensas, analytics, etc.

---

## 2. ARQUITETURA CONCEITUAL

### 2.1. Estrutura de 3 Camadas

```
┌─────────────────────────────────────────────────────────────────┐
│                     PLATFORM (Infraestrutura)                    │
│  - Multi-tenant engine                                          │
│  - Autenticação/autorização                                     │
│  - Billing/Payments                                             │
│  - CDN/Storage                                                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ acessa
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    MODO STUDIO (Ferramenta)                      │
│  - Interface de criação de InfoApps                             │
│  - Wizard de configuração                                       │
│  - Escolha de Estações/Dinâmicas/Tarefas                        │
│  - Preview e publicação                                         │
│  - Dashboard de InfoApps criados                                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ cria
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    InfoApp 1 (SaaS de Educação)                  │
│  ┌───────────────────────┐  ┌─────────────────────────────────┐│
│  │   LEARNER APP         │  │   InfoApp ADMIN PANEL           ││
│  │   (Aluno aprende)     │  │   (Criador gerencia)            ││
│  │                       │  │                                 ││
│  │  - Home/Missões       │  │  - Dashboard                    ││
│  │  - Player             │  │  - Gestão de Conteúdo (CRUD)    ││
│  │  - Review SRS         │  │  - Upload em Massa              ││
│  │  - Trilha             │  │  - Configurar Loja Recompensas  ││
│  │  - Progresso          │  │  - Usuários/Cohorts             ││
│  │  - Badges/Ligas       │  │  - Analytics                    ││
│  │  - Loja Recompensas   │  │  - Configurações (branding)     ││
│  │  - Perfil             │  │                                 ││
│  └───────────────────────┘  └─────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    InfoApp 2 (Outro SaaS completo)               │
│  ┌───────────────────────┐  ┌─────────────────────────────────┐│
│  │   LEARNER APP         │  │   InfoApp ADMIN PANEL           ││
│  └───────────────────────┘  └─────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘

                            ... InfoApp N ...
```

### 2.2. Componentes Principais

#### A. Platform (Infraestrutura)
**Função**: Camada técnica que suporta todos os InfoApps
**Responsabilidades**:
- Multi-tenancy (isolamento de dados)
- Autenticação/autorização
- Billing e pagamentos
- CDN, storage, cache
- Monitoramento e logs
- Infraestrutura global

**Usuários**: Nenhum usuário direto (camada técnica)

---

#### B. Modo Studio (Ferramenta de Criação)
**Função**: Interface onde criadores criam InfoApps
**Responsabilidades**:
- Wizard de criação de InfoApp
- Escolha de Estações/Dinâmicas/Tarefas
- Configuração inicial (nome, idioma, branding)
- Preview do InfoApp em emulador
- Publicação do InfoApp
- Dashboard listando InfoApps criados

**Usuários**: Criadores (Creators)

**Fluxo**:
1. Criador entra no Modo Studio
2. Clica em "Criar InfoApp"
3. Wizard: Nome, descrição, idioma principal, escolhe estações/dinâmicas
4. Preview do InfoApp
5. Publicar → InfoApp fica disponível em subdomínio ou domínio próprio
6. Criador acessa InfoApp Admin Panel para gerenciar conteúdo

---

#### C. InfoApp (SaaS de Educação)
**Função**: Produto final - SaaS de educação completo
**Responsabilidades**:
- **Learner App**: Interface onde alunos aprendem (gamificada, interativa)
- **InfoApp Admin Panel**: Interface onde criador gerencia conteúdo, loja, analytics

**Usuários**:
- **Learner App**: Alunos (Learners)
- **InfoApp Admin Panel**: Criador (dono do InfoApp)

**Características**:
- Isolado (dados, branding, subdomínio)
- Completo (não depende de outras apps para funcionar)
- Agnóstico de conteúdo (pode ser sobre qualquer tema)
- Gamificação nativa (XP, Coins, Badges, Ligas)
- Loja de Recompensas integrada

---

## 3. COMPONENTES DETALHADOS

### 3.1. Modo Studio

**O que é**: Ferramenta SaaS onde criadores constroem InfoApps

**Telas principais**:
1. **Dashboard**: Lista de InfoApps criados pelo criador
2. **Criar InfoApp (Wizard)**:
   - Nome, descrição
   - Idioma principal (PT-BR, EN, ES)
   - Escolher Estações (ex: Iniciante, Intermediário, Avançado)
   - Escolher Dinâmicas (ex: Missão Diária, Aula Interativa, Atividade, Review SRS)
   - Escolher Tarefas (ex: Match, Quiz, Simulação)
3. **Preview/Emulador**: Visualizar InfoApp antes de publicar
4. **Publicar**: Definir subdomínio (ex: meuapp.plataforma.com), publicar InfoApp
5. **Analytics**: Visão geral de todos os InfoApps criados
6. **Billing**: Plano do criador, pagamentos
7. **Settings**: Configurações da conta do criador

**O que NÃO faz**:
- ❌ Criar conteúdo detalhado (lessons, beats) → isso é no InfoApp Admin
- ❌ Configurar loja de recompensas → isso é no InfoApp Admin
- ❌ Gerenciar usuários do InfoApp → isso é no InfoApp Admin

**O que faz**:
- ✅ Criar estrutura do InfoApp (wizard)
- ✅ Escolher dinâmicas e tarefas
- ✅ Upload em massa inicial (opcional, para popular InfoApp)
- ✅ Preview e publicar

---

### 3.2. InfoApp - Learner App

**O que é**: Interface onde alunos aprendem (front-end do InfoApp)

**Telas principais**:
1. Home/Missão do Dia
2. Player (Aula Interativa com beats + checkpoints)
3. Conclusão (XP breakdown)
4. Review SRS (revisão espaçada)
5. Atividade Interativa (quiz/simulação) ← **RENOMEADO** de "Aplicação"
6. Trilha/Mapa
7. Progresso
8. Badges/Galeria
9. Ligas/Ranking
10. **Loja de Recompensas** (aluno gasta Coins)
11. Perfil
12. Configurações

**Características**:
- Gamificação robusta (XP, Coins, Badges, Streaks, Ligas)
- Interatividade (checkpoints, escolhas, simulações)
- Feedback imediato
- Multilíngue (interface + áudio TTS)
- Acessibilidade (modo escuro, fontes ajustáveis, narração)

---

### 3.3. InfoApp - Admin Panel

**O que é**: Interface onde criador gerencia o InfoApp criado

**Telas principais**:
1. **Dashboard**: Visão geral (alunos ativos, conclusão, engajamento)
2. **Gestão de Conteúdo**:
   - CRUD de Lessons, Beats, Checkpoints
   - Editor visual de Aulas Interativas
   - Biblioteca de mídia (imagens, áudios, vídeos)
3. **Upload em Massa**:
   - CSV/JSON/YAML upload
   - Validação e importação de conteúdo
4. **Configurar Loja de Recompensas**: ← **MOVIDO DO MODO STUDIO**
   - CRUD de produtos (desconto, físico, digital, personalização)
   - Definir preços em Coins
   - Gestão de inventário
   - Histórico de compras
5. **Usuários/Cohorts**:
   - Lista de alunos
   - Cohorts (turmas)
   - Permissões
6. **Analytics**:
   - Engajamento, conclusão, retenção
   - Funil de aprendizado
   - Heatmaps de dificuldade
7. **Configurações**:
   - Branding (logo, cores, tema)
   - Idioma padrão
   - Domínio/subdomínio
   - Integrações (Webhooks, API)

**Características**:
- Interface admin moderna (tipo Retool/Supabase)
- Bulk operations
- Versionamento de conteúdo
- Auditoria de mudanças

---

## 4. MOTOR: ESTAÇÕES → DINÂMICAS → TAREFAS

**Função**: Motor que transforma conteúdo bruto em SaaS de educação interativo

### Como funciona:
1. **Criador fornece conteúdo** (ebook, PDF, vídeo, texto)
2. **Modo Studio processa usando Estações/Dinâmicas/Tarefas**:
   - **Estações**: Níveis/módulos (ex: Iniciante, Intermediário, Avançado)
   - **Dinâmicas**: Tipos de atividades (Missão, Aula, Atividade, Review)
   - **Tarefas**: Interações específicas (Match, Quiz, Simulação, Recall)
3. **InfoApp Learner exibe conteúdo** gamificado e interativo

**Exemplo**:
- **Input**: Ebook "Introdução a Python" (PDF)
- **Processamento**:
  - Estação: "Fundamentos" → 5 lessons
  - Dinâmica: "Aula Interativa" → Cada lesson tem 3-5 beats
  - Tarefas: Quiz (múltipla escolha), Code Match (arrastar código), Simulação (REPL)
- **Output**: InfoApp "Aprenda Python" com 5 aulas interativas gamificadas

---

## 5. PLATFORM ADMIN - DECISÃO

### Decisão arquitetural:
**Platform Admin NÃO existe como app separado** (confirmado pelo cliente)

### Opções:
1. **Opção A (RECOMENDADA)**: Platform Admin vira **nível de acesso no Modo Studio**
   - Super Admin (operador da plataforma) entra no Modo Studio
   - Vê todos os InfoApps criados (de todos os criadores)
   - Pode moderar, suspender, auditar
   - Interface: Modo Studio com permissões expandidas

2. **Opção B**: Platform Admin é removido completamente
   - Operador da plataforma usa ferramentas internas (admin Django, SQL, logs)
   - Sem interface visual

**Cliente disse**: "acho que podemos matar a ideia de admin, a não ser que seja a nível de acesso"

→ **Implementar Opção A**: Platform Admin é nível de acesso (role: super_admin) no Modo Studio

---

## 6. FLUXO COMPLETO (USER JOURNEY)

### 6.1. Criador cria InfoApp

```
1. Criador acessa plataforma.com/studio
2. Login/Signup
3. Dashboard: "Criar InfoApp"
4. Wizard:
   - Nome: "Aprenda Python"
   - Descrição: "Curso interativo de Python para iniciantes"
   - Idioma: PT-BR
   - Estações: Fundamentos, Intermediário, Avançado
   - Dinâmicas: Missão Diária, Aula Interativa, Quiz, Review SRS
5. Upload em massa (opcional): CSV com lessons/beats
6. Preview no emulador
7. Publicar:
   - Subdomínio: "aprenda-python.plataforma.com"
   - Publicar ✅
8. InfoApp criado!
9. Criador acessa "aprenda-python.plataforma.com/admin"
10. InfoApp Admin Panel:
    - Criar lessons manualmente
    - Configurar loja de recompensas
    - Convidar primeiros alunos
```

### 6.2. Aluno usa InfoApp

```
1. Aluno acessa "aprenda-python.plataforma.com"
2. Signup (cria conta no InfoApp)
3. Onboarding: Define objetivo de aprendizado
4. Home: Vê Missão do Dia
5. Player: Completa Aula Interativa (beats + checkpoints)
6. Conclusão: Ganha XP + Coins
7. Review SRS: Revisa conceitos anteriores
8. Atividade Interativa: Completa quiz (ganha Coins extras por acerto)
9. Progresso: Vê badges desbloqueados
10. Loja: Gasta Coins em desconto ou personalização (tema escuro premium)
```

### 6.3. Criador gerencia InfoApp

```
1. Criador acessa "aprenda-python.plataforma.com/admin"
2. Dashboard: 50 alunos ativos, 70% conclusão
3. Gestão de Conteúdo:
   - Criar nova lesson "Funções em Python"
   - Adicionar 4 beats (conceito, exemplo, prática, resumo)
   - Adicionar checkpoints
4. Configurar Loja:
   - Criar produto "Certificado Premium" (100 Coins)
   - Criar produto "Tema Escuro" (50 Coins)
5. Analytics: Heatmap mostra lesson 3 tem 40% abandono → ajustar conteúdo
6. Usuários: Criar cohort "Turma Jan/2025"
```

---

## 7. ISOLAMENTO E MULTI-TENANCY

### Cada InfoApp é isolado:
- **Dados**: Usuários, lessons, progresso são isolados por InfoApp
- **Branding**: Logo, cores, tema próprios
- **Domínio**: Subdomínio ou domínio próprio
- **Loja**: Cada InfoApp tem sua loja de recompensas

### Multi-tenancy técnico:
Ver documento `13_ARQUITETURA_PRODUTO/arquitetura_tecnica.md` para decisão sobre:
- Shared DB com tenant_id (Opção A)
- Isolated DB por InfoApp (Opção B)
- Hybrid (Opção C)

---

## 8. MUDANÇAS CRÍTICAS vs ARQUITETURA ANTERIOR

| Aspecto | ANTES (errado) | DEPOIS (correto) |
|---------|----------------|------------------|
| **Platform Admin** | App separado | Nível de acesso no Modo Studio |
| **Loja de Recompensas** | Configurada no Creator Studio | Configurada no InfoApp Admin Panel |
| **"Aplicação"** | Tarefa de mundo real com upload | Atividade Interativa (quiz/simulação) |
| **Upload em Massa** | Modo Studio | InfoApp Admin Panel (+ opcional no Studio) |
| **Multi-idioma** | v1.1 (futuro) | v1 (obrigatório) |
| **Conceito** | Plataforma de cursos | Gerador de EdTech SaaS |
| **Estrutura** | Learner + Creator + Admin | Studio → InfoApp (Learner + Admin) |

---

## 9. GLOSSÁRIO

| Termo | Definição |
|-------|-----------|
| **Platform** | Infraestrutura técnica (multi-tenant, auth, billing) |
| **Modo Studio** | Ferramenta onde criadores criam InfoApps |
| **InfoApp** | SaaS de educação completo (produto final) |
| **Learner App** | Front-end do InfoApp (onde aluno aprende) |
| **InfoApp Admin Panel** | Back-office do InfoApp (onde criador gerencia) |
| **Criador** | Pessoa que cria InfoApp usando Modo Studio |
| **Aluno** | Pessoa que aprende usando Learner App |
| **Operador** | Super Admin (gerencia plataforma, nível de acesso) |
| **Estação** | Nível/módulo de conteúdo (ex: Iniciante, Avançado) |
| **Dinâmica** | Tipo de atividade (Missão, Aula, Quiz, Review) |
| **Tarefa** | Interação específica (Match, Quiz, Simulação) |
| **Beat** | Unidade mínima de conteúdo (3-12 seg de áudio) |
| **Checkpoint** | Verificação de aprendizado (múltipla escolha, match) |
| **XP** | Experiência (progresso, sobe nível, NÃO se gasta) |
| **Coins** | Moeda (gasta na loja de recompensas) |

---

## 10. PRÓXIMOS PASSOS

1. ✅ Definir arquitetura técnica multi-tenant (ver `arquitetura_tecnica.md`)
2. ✅ Criar specs detalhadas do InfoApp Admin Panel
3. ✅ Atualizar specs do Modo Studio (foco em criação)
4. ✅ Atualizar specs do Learner App (renomear "Aplicação")
5. ✅ Definir sistema de gamificação (XP vs Coins, bonificações)
6. ✅ Definir estratégia multi-idioma (i18n + TTS)
7. 🔄 Validar arquitetura com cliente (ver `16_CONFIRMACOES_CLIENTE/perguntas_criticas.md`)

---

**Revisado por**: Product Architect
**Aprovado por**: [Aguardando validação do cliente]
