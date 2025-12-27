# GitHub Completo (Repository + Pages + Actions + Codespaces)

**Versão**: 1.0 (NOVO)
**Data**: 2025-12-27
**Status**: OFICIAL (baseado em resposta final do cliente)
**Mudança Crítica**: Usar TODAS funcionalidades GitHub (código + docs + CI/CD + dev cloud)

---

## RESPOSTA FINAL DO CLIENTE

✅ **"Todas as opções que forem necessárias e mais adequadas"**

**IMPLEMENTAR**:
- **GitHub Repository**: Código open source (MIT/GPL)
- **GitHub Pages**: Documentação + landing page
- **GitHub Actions**: CI/CD automático (commit → testes → deploy)
- **GitHub Codespaces**: VS Code na nuvem (dev sem instalar nada)

---

## 1. GITHUB REPOSITORY (CÓDIGO OPEN SOURCE)

### 1.1. Estrutura do Repositório

```
KML_InfoApp/
├── .github/
│   ├── workflows/           # GitHub Actions (CI/CD)
│   │   ├── ci.yml           # Testes automáticos
│   │   ├── deploy.yml       # Deploy automático
│   │   └── preview.yml      # Preview branches (PR)
│   ├── ISSUE_TEMPLATE/      # Templates para issues
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── devcontainer.json    # GitHub Codespaces config
│
├── apps/
│   ├── learner/             # Frontend aluno (Next.js)
│   ├── creator-studio/      # Frontend criador (Next.js)
│   └── platform-admin/      # Frontend admin (Next.js)
│
├── backend/
│   ├── api/                 # Backend API (FastAPI)
│   ├── services/            # Services (TTS, Storage, etc.)
│   └── database/            # Schemas, migrations
│
├── docs/                    # Documentação (GitHub Pages)
│   ├── index.md             # Landing page
│   ├── getting-started.md   # Setup rápido
│   ├── architecture.md      # Arquitetura técnica
│   └── api/                 # Documentação API
│
├── docker/
│   ├── docker-compose.yml   # Localhost setup
│   ├── Dockerfile.backend   # Backend container
│   └── Dockerfile.frontend  # Frontend container
│
├── scripts/
│   ├── setup.sh             # Setup inicial (1 comando)
│   ├── test.sh              # Rodar testes
│   └── deploy.sh            # Deploy manual
│
├── .env.example             # Variáveis de ambiente (template)
├── README.md                # README principal
├── LICENSE                  # Licença MIT/GPL
├── CONTRIBUTING.md          # Como contribuir
└── CHANGELOG.md             # Histórico de versões
```

---

### 1.2. Licença Open Source

**Opções de licença**:

**Opção A: MIT License** (recomendado)
- ✅ Permissivo (pode usar comercialmente)
- ✅ Simples
- ✅ Compatível com maioria dos projetos
- ❌ Não garante open source (pode ser fechado por quem fork)

**Opção B: GPL v3**
- ✅ Copyleft (fork precisa ser open source)
- ✅ Protege contra apropriação comercial fechada
- ❌ Mais restritiva
- ❌ Incompatível com alguns projetos proprietários

**Opção C: AGPL v3** (GPL para web)
- ✅ Copyleft forte (mesmo uso via API precisa ser open source)
- ✅ Ideal para SaaS
- ❌ Muito restritiva (espanta contribuidores comerciais)

**Recomendação**: **MIT License** (v1) → Foco em adoção e comunidade

**Arquivo LICENSE**:
```
MIT License

Copyright (c) 2025 [Seu Nome/Empresa]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

[...resto da licença MIT...]
```

---

### 1.3. README.md (Porta de Entrada)

**README completo**:
```markdown
# InfoApp Platform

> Plataforma open source para criar apps de aprendizagem ativa com gamificação, baseada em ciências cognitivas.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CI Status](https://github.com/seu-usuario/infoapp/workflows/CI/badge.svg)](https://github.com/seu-usuario/infoapp/actions)
[![Deploy](https://img.shields.io/badge/deploy-GCP-blue)](https://infoapp.com)

---

## 🚀 O que é?

InfoApp é uma plataforma que permite **criadores de conteúdo** construírem apps de aprendizagem interativos, com:

- ✅ **Aprendizagem Ativa**: Missões, checkpoints, aplicação prática
- ✅ **Gamificação**: Coins, badges, streaks, loja de recompensas
- ✅ **Retenção**: SRS (Spaced Repetition System) nativo
- ✅ **Multi-idioma**: PT-BR, EN-US, ES-ES
- ✅ **Open Source**: MIT License, self-hosted

**Demo**: [https://demo.infoapp.com](https://demo.infoapp.com)
**Docs**: [https://docs.infoapp.com](https://docs.infoapp.com)

---

## 📦 Setup Rápido (Localhost)

**Requisitos**:
- Docker 20+
- Docker Compose 2+
- Git

**1 comando**:
```bash
git clone https://github.com/seu-usuario/infoapp.git
cd infoapp
./scripts/setup.sh
```

Abra: `http://localhost:3000` (Learner App)
Admin: `http://localhost:3001` (Creator Studio)

---

## 🏗️ Stack Técnica

- **Frontend**: Next.js 14 (App Router) + TypeScript + Tailwind CSS
- **Backend**: FastAPI (Python 3.11) + PostgreSQL 15
- **Infra**: Docker + Docker Compose (localhost) | GCP (cloud)
- **TTS**: ElevenLabs API (cache inteligente)
- **Pagamentos**: Stripe Connect (opcional)
- **Analytics**: Google Analytics 4 (abstração de eventos)

---

## 📚 Documentação

- [Getting Started](./docs/getting-started.md)
- [Arquitetura](./docs/architecture.md)
- [API Reference](./docs/api/)
- [Como Contribuir](./CONTRIBUTING.md)

---

## 🤝 Como Contribuir

Contribuições são bem-vindas! Veja [CONTRIBUTING.md](./CONTRIBUTING.md)

**Issues abertas**: [GitHub Issues](https://github.com/seu-usuario/infoapp/issues)
**Roadmap**: [Roadmap Público](./docs/roadmap.md)

---

## 📄 Licença

MIT License - veja [LICENSE](./LICENSE)

---

## 🙏 Créditos

Criado por [Seu Nome] | Inspirado em ciências cognitivas e gamificação
```

---

## 2. GITHUB PAGES (DOCUMENTAÇÃO + LANDING PAGE)

### 2.1. Ativar GitHub Pages

**Passo 1: Criar branch `gh-pages`**
```bash
git checkout --orphan gh-pages
git rm -rf .
echo "# InfoApp Docs" > index.md
git add index.md
git commit -m "Initial docs"
git push origin gh-pages
```

**Passo 2: Configurar no GitHub**
1. Vai em: Settings → Pages
2. Source: Deploy from a branch
3. Branch: `gh-pages` / `root`
4. Salvar

**URL**: `https://seu-usuario.github.io/infoapp/`

---

### 2.2. Estrutura de Documentação (MkDocs)

**Usar MkDocs Material** (framework de docs estático)

**Instalar**:
```bash
pip install mkdocs-material
```

**Config `mkdocs.yml`**:
```yaml
site_name: InfoApp Platform
site_description: Plataforma open source de aprendizagem ativa
site_url: https://seu-usuario.github.io/infoapp/
repo_url: https://github.com/seu-usuario/infoapp
repo_name: infoapp

theme:
  name: material
  palette:
    primary: indigo
    accent: pink
  features:
    - navigation.tabs
    - navigation.sections
    - toc.integrate
    - search.suggest

nav:
  - Home: index.md
  - Getting Started:
    - Instalação: getting-started/installation.md
    - Localhost: getting-started/localhost.md
    - Deploy GCP: getting-started/deploy-gcp.md
  - Arquitetura:
    - Visão Geral: architecture/overview.md
    - Frontend: architecture/frontend.md
    - Backend: architecture/backend.md
    - Database: architecture/database.md
  - API Reference:
    - Learner API: api/learner.md
    - Creator API: api/creator.md
    - Admin API: api/admin.md
  - Contribuindo:
    - Como Contribuir: contributing/how-to.md
    - Código de Conduta: contributing/code-of-conduct.md

markdown_extensions:
  - admonition
  - codehilite
  - pymdownx.highlight
  - pymdownx.superfences
```

**Build docs**:
```bash
mkdocs build
```

**Deploy automático** (GitHub Actions):
```yaml
# .github/workflows/docs.yml
name: Deploy Docs

on:
  push:
    branches: [main]
    paths:
      - 'docs/**'
      - 'mkdocs.yml'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.11
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```

**Resultado**: Commit em `docs/` → GitHub Actions → Build MkDocs → Deploy em `gh-pages`

---

### 2.3. Landing Page (Alternativa)

**Se não usar MkDocs**: Landing page HTML estática

**`docs/index.html`**:
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InfoApp Platform - Aprendizagem Ativa Open Source</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-50">
    <nav class="bg-white shadow">
        <div class="max-w-7xl mx-auto px-4 py-4">
            <h1 class="text-2xl font-bold text-indigo-600">InfoApp Platform</h1>
        </div>
    </nav>

    <header class="bg-gradient-to-r from-indigo-500 to-purple-600 text-white py-20">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <h2 class="text-5xl font-bold mb-4">Aprendizagem Ativa, Open Source</h2>
            <p class="text-xl mb-8">Crie apps de aprendizagem interativos com gamificação baseada em ciências cognitivas</p>
            <a href="https://github.com/seu-usuario/infoapp" class="bg-white text-indigo-600 px-8 py-3 rounded-lg font-semibold hover:bg-gray-100">
                Ver no GitHub →
            </a>
        </div>
    </header>

    <section class="max-w-7xl mx-auto px-4 py-16">
        <div class="grid md:grid-cols-3 gap-8">
            <div class="bg-white p-6 rounded-lg shadow">
                <h3 class="text-xl font-bold mb-2">🚀 Setup Rápido</h3>
                <p>1 comando Docker Compose e você tem um InfoApp rodando localhost</p>
            </div>
            <div class="bg-white p-6 rounded-lg shadow">
                <h3 class="text-xl font-bold mb-2">🎮 Gamificação</h3>
                <p>Coins, badges, streaks, loja de recompensas out-of-the-box</p>
            </div>
            <div class="bg-white p-6 rounded-lg shadow">
                <h3 class="text-xl font-bold mb-2">🧠 Ciência</h3>
                <p>SRS nativo, checkpoints cognitivos, aplicação prática</p>
            </div>
        </div>
    </section>

    <footer class="bg-gray-800 text-white py-8 mt-16">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <p>MIT License | <a href="https://github.com/seu-usuario/infoapp" class="text-indigo-400">GitHub</a></p>
        </div>
    </footer>
</body>
</html>
```

**Deploy**: Commit `docs/index.html` → GitHub Pages serve automaticamente

---

## 3. GITHUB ACTIONS (CI/CD AUTOMÁTICO)

### 3.1. Workflow: CI (Testes Automáticos)

**Arquivo**: `.github/workflows/ci.yml`

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          cd backend
          pip install -r requirements.txt
          pip install pytest pytest-cov

      - name: Run tests
        run: |
          cd backend
          pytest tests/ --cov=api --cov-report=xml

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./backend/coverage.xml

  test-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: |
          cd apps/learner
          npm ci

      - name: Run tests
        run: |
          cd apps/learner
          npm run test

      - name: Build
        run: |
          cd apps/learner
          npm run build

  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Lint Backend (Ruff)
        run: |
          pip install ruff
          ruff check backend/

      - name: Lint Frontend (ESLint)
        run: |
          cd apps/learner
          npm ci
          npm run lint
```

**Resultado**: Commit → GitHub Actions roda testes → PR só merge se passar

---

### 3.2. Workflow: Deploy Automático (GCP)

**Arquivo**: `.github/workflows/deploy.yml`

```yaml
name: Deploy to GCP

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Authenticate to GCP
        uses: google-github-actions/auth@v1
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}

      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v1

      - name: Build and Push Docker Image (Backend)
        run: |
          gcloud builds submit \
            --tag gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend:${{ github.sha }} \
            --file docker/Dockerfile.backend

      - name: Deploy to Cloud Run (Backend)
        run: |
          gcloud run deploy backend \
            --image gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend:${{ github.sha }} \
            --platform managed \
            --region us-central1 \
            --allow-unauthenticated

      - name: Build and Deploy Frontend (Vercel)
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./apps/learner
          vercel-args: '--prod'
```

**Secrets necessários** (GitHub Settings → Secrets):
- `GCP_SA_KEY`: Service Account JSON (GCP)
- `GCP_PROJECT_ID`: ID do projeto GCP
- `VERCEL_TOKEN`: Token de deploy Vercel (frontend)

**Resultado**: Push em `main` → Deploy automático GCP + Vercel

---

### 3.3. Workflow: Preview Branches (PR)

**Arquivo**: `.github/workflows/preview.yml`

```yaml
name: Preview Deploy (PR)

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Deploy Preview (Vercel)
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./apps/learner
          alias-domains: pr-${{ github.event.pull_request.number }}.infoapp.vercel.app

      - name: Comment PR with preview URL
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '🚀 Preview deploy: https://pr-${{ github.event.pull_request.number }}.infoapp.vercel.app'
            })
```

**Resultado**: PR aberto → GitHub Actions cria preview URL → Comenta no PR

---

## 4. GITHUB CODESPACES (DEV NA NUVEM)

### 4.1. Configurar Devcontainer

**Arquivo**: `.github/devcontainer.json`

```json
{
  "name": "InfoApp Dev Environment",
  "dockerComposeFile": "../docker/docker-compose.dev.yml",
  "service": "app",
  "workspaceFolder": "/workspace",

  "features": {
    "ghcr.io/devcontainers/features/python:1": {
      "version": "3.11"
    },
    "ghcr.io/devcontainers/features/node:1": {
      "version": "18"
    },
    "ghcr.io/devcontainers/features/docker-in-docker:2": {}
  },

  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python",
        "ms-python.vscode-pylance",
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "bradlc.vscode-tailwindcss",
        "GitHub.copilot"
      ],
      "settings": {
        "python.defaultInterpreterPath": "/usr/local/bin/python",
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
        }
      }
    }
  },

  "forwardPorts": [3000, 3001, 8000, 5432],
  "portsAttributes": {
    "3000": { "label": "Learner App" },
    "3001": { "label": "Creator Studio" },
    "8000": { "label": "Backend API" },
    "5432": { "label": "PostgreSQL" }
  },

  "postCreateCommand": "bash .devcontainer/setup.sh",

  "remoteUser": "vscode"
}
```

**Script de setup** (`.devcontainer/setup.sh`):
```bash
#!/bin/bash

# Install backend dependencies
cd /workspace/backend
pip install -r requirements.txt

# Install frontend dependencies
cd /workspace/apps/learner
npm install

cd /workspace/apps/creator-studio
npm install

# Setup database
cd /workspace
docker-compose -f docker/docker-compose.dev.yml up -d postgres
sleep 5
cd backend
python -m alembic upgrade head

echo "✅ Dev environment ready!"
```

---

### 4.2. Como Usar Codespaces

**Passo 1: Abrir Codespace**
1. Vai no repositório GitHub
2. Clica "Code" → "Codespaces" → "Create codespace on main"
3. GitHub cria VM na nuvem com VS Code

**Passo 2: Desenvolver**
- VS Code abre no navegador
- Terminal integrado
- Extensões instaladas automaticamente
- Backend + Frontend + DB rodando

**Passo 3: Testar**
- Abrir `http://localhost:3000` (forward automático)
- Fazer mudanças → hot reload

**Custo**:
- Grátis: 60h/mês (2 cores)
- Depois: $0.18/hora (2 cores)

**Vantagem**:
- Dev sem instalar nada
- Ambiente consistente (todos devs usam mesmo setup)
- Onboarding 1-click (novo dev começa em 2 min)

---

## 5. FLUXO COMPLETO (EXEMPLO)

### 5.1. Contribuidor Externo Contribuindo

**Passo 1: Fork repositório**
```bash
# No GitHub: Fork → seu-usuario/infoapp
git clone https://github.com/seu-usuario/infoapp.git
```

**Passo 2: Abrir no Codespaces** (opcional)
- Clica "Code" → "Codespaces" → "Create codespace"
- VS Code abre, setup automático

**Passo 3: Criar branch**
```bash
git checkout -b feature/add-dark-mode
```

**Passo 4: Desenvolver**
```bash
cd apps/learner
# Fazer mudanças...
npm run dev  # Testar localhost
```

**Passo 5: Commit**
```bash
git add .
git commit -m "feat: add dark mode toggle"
git push origin feature/add-dark-mode
```

**Passo 6: Abrir PR**
- No GitHub: "Compare & pull request"
- Preenche descrição
- GitHub Actions roda CI → Testes passam ✅
- Preview deploy criado → URL comentada no PR
- Mantenedor revisa → Merge

**Passo 7: Deploy automático**
- PR merged em `main`
- GitHub Actions: Deploy to GCP
- Produção atualizada em 5 min

---

## 6. BENEFÍCIOS DO SETUP COMPLETO

**Para Contribuidores**:
- ✅ Setup 1-click (Codespaces ou `./scripts/setup.sh`)
- ✅ CI/CD garante qualidade (testes automáticos)
- ✅ Preview deploy (testar antes de merge)
- ✅ Documentação sempre atualizada (GitHub Pages)

**Para Mantenedores**:
- ✅ Deploy automático (sem trabalho manual)
- ✅ Testes obrigatórios (PRs só mergem se passar)
- ✅ Rastreabilidade (changelog automático, tags)
- ✅ Comunidade engajada (issues, PRs bem organizados)

**Para Usuários Finais**:
- ✅ Documentação clara (GitHub Pages)
- ✅ Instalação fácil (1 comando Docker)
- ✅ Código auditável (open source no GitHub)
- ✅ Atualizações frequentes (CI/CD rápido)

---

## 7. CUSTOS

**GitHub**:
- ✅ Repository público: GRÁTIS
- ✅ GitHub Pages: GRÁTIS
- ✅ GitHub Actions: 2.000 min/mês grátis (depois $0.008/min)
- ✅ GitHub Codespaces: 60h/mês grátis (depois $0.18/h)

**Estimativa v1** (50 commits/mês, 10 PRs/mês):
- Actions: ~500 min/mês → GRÁTIS (dentro free tier)
- Codespaces: ~20h/mês (1 dev) → GRÁTIS
- **Total: R$ 0/mês**

---

## 8. PRÓXIMOS PASSOS

**v1**:
- [x] Definir estrutura GitHub completa (COMPLETO)
- [ ] Criar repositório GitHub
- [ ] Setup inicial: README, LICENSE, CONTRIBUTING
- [ ] Configurar GitHub Pages (MkDocs)
- [ ] Criar workflows: CI, Deploy, Preview
- [ ] Configurar Codespaces (devcontainer.json)
- [ ] Primeira release (v1.0.0)

**v1.1**:
- [ ] Integração Dependabot (atualizar deps automaticamente)
- [ ] Integração CodeQL (security scanning)
- [ ] Badges no README (build status, coverage, etc.)
- [ ] Changelog automático (conventional commits)

---

**Criado por**: DevOps Lead + Community Manager
**Baseado em**: Resposta final do cliente (2025-12-27)
**Aprovado por**: Cliente ✅
**Relacionado**: `localhost_docker.md`, `deploy_gcp.md`
