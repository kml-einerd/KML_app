# Arquitetura Google Cloud Platform (GCP)

**Versão**: 1.0
**Data**: 2025-12-27
**Status**: PROPOSTA (aguardando validação cliente)
**Contexto**: Arquitetura para hospedar plataforma open source no Google Cloud

---

## 1. VISÃO GERAL

### 1.1. Decisões Fundamentais

**Cliente disse**:
- ✅ "Roda em servidor cloud (GCP)"
- ✅ "Grátis para todos (open source)"
- ✅ "Quero conseguir visualizar e testar localmente"

**Nossa proposta**:
- **Código open source** no GitHub (público)
- **Pode rodar em**:
  - **Localhost** (desenvolvimento/testes) → Docker Compose
  - **Google Cloud** (produção) → Cloud Run + Cloud SQL
  - **GitHub Actions** (CI/CD) → Deploy automático

---

## 2. ARQUITETURA GCP (PRODUÇÃO)

### 2.1. Diagrama de Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                        GOOGLE CLOUD                          │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │ Cloud Run    │◄────────│ Load Balancer│◄──── Internet    │
│  │ (Frontend)   │         │              │                   │
│  │ Next.js/React│         └──────────────┘                   │
│  └──────────────┘                                            │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │ Cloud Run    │◄────────│ Cloud SQL    │                  │
│  │ (Backend)    │         │ PostgreSQL   │                  │
│  │ FastAPI/Django         │              │                  │
│  └──────────────┘         └──────────────┘                  │
│         │                        │                           │
│         ▼                        ▼                           │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │ Cloud Storage│         │ Cloud Functions│                │
│  │ (Assets)     │         │ (TTS ElevenLabs)│               │
│  └──────────────┘         └──────────────┘                  │
│                                                               │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │ Cloud Logging│         │ Cloud Monitor│                  │
│  │              │         │              │                   │
│  └──────────────┘         └──────────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES GCP

### 3.1. Cloud Run (Frontend)

**Serviço**: Frontend (React/Next.js)

**Características**:
- Serverless (auto-scale 0 → N instâncias)
- Paga apenas por requisição (não paga quando ocioso)
- Deploy via Docker container

**Configuração**:
```yaml
service: frontend
region: southamerica-east1  # São Paulo
min_instances: 0  # Escala para 0 quando sem tráfego
max_instances: 10  # Máximo de instâncias simultâneas
cpu: 1
memory: 512Mi
timeout: 60s
```

**Custo estimado**:
- Free tier: 2 milhões requests/mês
- Após free tier: ~$0.40 por milhão de requests
- **Estimativa v1**: Grátis (dentro do free tier)

---

### 3.2. Cloud Run (Backend)

**Serviço**: Backend (FastAPI/Django)

**Características**:
- Serverless (auto-scale)
- Conexão segura com Cloud SQL (via Unix socket)

**Configuração**:
```yaml
service: backend
region: southamerica-east1
min_instances: 1  # Mantém 1 instância sempre (evita cold start)
max_instances: 20
cpu: 2
memory: 1Gi
timeout: 300s
env_vars:
  - DATABASE_URL: postgresql://...
  - ELEVENLABS_API_KEY: ...
```

**Custo estimado**:
- Free tier: 2 milhões requests/mês
- 1 instância sempre ligada: ~$10-15/mês
- **Estimativa v1**: ~$10-15/mês

---

### 3.3. Cloud SQL (PostgreSQL)

**Serviço**: Banco de dados relacional

**Características**:
- Managed PostgreSQL
- Backups automáticos
- Alta disponibilidade (opcional)

**Configuração recomendada v1**:
```yaml
tier: db-f1-micro  # Free tier
region: southamerica-east1
storage: 10 GB
backup: daily
high_availability: false  # v1 não precisa (custo alto)
```

**Custo estimado**:
- Free tier: db-f1-micro (1 instância compartilhada)
- Limitações free tier:
  - 1 vCPU compartilhada
  - 614 MB RAM
  - 10 GB storage
- **Estimativa v1**: Grátis (free tier) se < 10GB dados
- Após free tier: ~$10-30/mês (db-g1-small)

**Upgrade path** (quando necessário):
- v1: db-f1-micro (grátis)
- v1.1: db-g1-small (~$25/mês) - mais RAM
- v2: db-n1-standard-1 (~$50/mês) - produção

---

### 3.4. Cloud Storage (Assets)

**Serviço**: Armazenamento de arquivos

**Uso**:
- Assets (imagens, áudios TTS gerados)
- Uploads de criadores (logos, produtos da loja)

**Configuração**:
```yaml
bucket: infoapp-assets
region: southamerica-east1
storage_class: STANDARD  # Acesso frequente
lifecycle:
  - delete_after_days: 365  # Limpar assets antigos
```

**Custo estimado**:
- Free tier: 5 GB storage + 5.000 requisições/mês
- Após free tier: $0.020 per GB/mês
- **Estimativa v1**: ~$5/mês (250 GB assets TTS)

---

### 3.5. Cloud Functions (TTS ElevenLabs)

**Serviço**: Função serverless para gerar TTS

**Por que usar Cloud Functions**:
- Isola chamadas de API externa (ElevenLabs)
- Retry automático em caso de falha
- Timeout maior (até 9 min)

**Configuração**:
```yaml
function: generate_tts
runtime: python310
region: southamerica-east1
timeout: 540s  # 9 min
memory: 256Mi
env_vars:
  - ELEVENLABS_API_KEY: ...
trigger: http
```

**Fluxo**:
1. Backend chama Cloud Function com texto
2. Function chama ElevenLabs API
3. Function faz upload do áudio para Cloud Storage
4. Function retorna URL do áudio

**Custo estimado**:
- Free tier: 2 milhões invocações/mês
- **Estimativa v1**: Grátis (dentro do free tier)

---

### 3.6. Cloud Logging + Monitoring

**Serviços**:
- **Cloud Logging**: Logs centralizados (erros, requests)
- **Cloud Monitoring**: Métricas (CPU, RAM, latência)

**Configuração**:
- Logs retidos por 30 dias (padrão)
- Alertas configurados:
  - Error rate > 5%
  - Latência > 2s (p95)
  - Custo mensal > $100

**Custo estimado**:
- Free tier: 50 GB logs/mês
- **Estimativa v1**: Grátis (dentro do free tier)

---

## 4. CUSTO TOTAL ESTIMADO

### 4.1. Breakdown Mensal (após free tier)

| Serviço | v1 (MVP) | v1.1 (Crescimento) | v2 (Escala) |
|---------|----------|-------------------|-------------|
| **Cloud Run (Frontend)** | Grátis | $5/mês | $20/mês |
| **Cloud Run (Backend)** | $10-15/mês | $30/mês | $100/mês |
| **Cloud SQL** | Grátis (free tier) | $25/mês | $50-100/mês |
| **Cloud Storage** | $5/mês | $15/mês | $50/mês |
| **Cloud Functions (TTS)** | Grátis | $5/mês | $20/mês |
| **ElevenLabs API** | $5/mês | $30/mês | $99/mês |
| **Logging/Monitoring** | Grátis | Grátis | $10/mês |
| **TOTAL** | **~$20-25/mês** | **~$110/mês** | **~$350/mês** |

### 4.2. Free Tier GCP (12 meses)

**Google Cloud oferece**:
- $300 USD crédito grátis (válido por 90 dias)
- Always Free tier (permanente):
  - Cloud Run: 2 milhões requests/mês
  - Cloud Functions: 2 milhões invocações/mês
  - Cloud SQL: 1 instância db-f1-micro
  - Cloud Storage: 5 GB

**Duração do free tier**:
- **Crédito $300**: 3 meses
- **Always Free**: Permanente (mas com limites)

**Após free tier acabar**: Paga conforme uso (pay-as-you-go)

---

## 5. ALTERNATIVAS DE HOSPEDAGEM

### 5.1. Self-Hosted (Cada criador hospeda próprio)

**Modelo**: Código open source, cada criador clona e hospeda

**Vantagens**:
- Cliente não paga nada (cada criador paga próprio servidor)
- Criadores têm controle total
- Compliance facilitado (dados do criador ficam no servidor dele)

**Desvantagens**:
- Setup complexo (criador precisa conhecimento técnico)
- Sem suporte centralizado
- Criadores podem usar infraestrutura diferente (AWS, DigitalOcean, etc.)

**Recomendação**: Fornecer guias detalhados para:
- GCP (recomendado)
- AWS (alternativa)
- DigitalOcean (mais simples, mais barato)
- Heroku (mais caro, mais fácil)

---

### 5.2. Localhost (Desenvolvimento)

**Modelo**: Rodar tudo localmente com Docker Compose

**Configuração**:
```yaml
# docker-compose.yml
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://localhost:8000

  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/infoapp
      - ELEVENLABS_API_KEY=${ELEVENLABS_API_KEY}

  db:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=infoapp
    volumes:
      - ./data/postgres:/var/lib/postgresql/data

  redis:
    image: redis:7
    ports:
      - "6379:6379"
```

**Comandos**:
```bash
# Clonar repositório
git clone https://github.com/seu-usuario/infoapp-platform.git
cd infoapp-platform

# Configurar variáveis de ambiente
cp .env.example .env
# Editar .env e adicionar ELEVENLABS_API_KEY

# Subir tudo
docker-compose up -d

# Acessar
# Frontend: http://localhost:3000
# Backend: http://localhost:8000
# Docs: http://localhost:8000/docs
```

**Custo**: $0 (gratuito)

---

## 6. CI/CD COM GITHUB ACTIONS

### 6.1. Workflow Automático

**Arquivo**: `.github/workflows/deploy.yml`

```yaml
name: Deploy to GCP

on:
  push:
    branches:
      - main  # Deploy automático ao push na main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@v1
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}

      - name: Build and push Docker images
        run: |
          gcloud builds submit --tag gcr.io/$PROJECT_ID/frontend ./frontend
          gcloud builds submit --tag gcr.io/$PROJECT_ID/backend ./backend

      - name: Deploy to Cloud Run
        run: |
          gcloud run deploy frontend \
            --image gcr.io/$PROJECT_ID/frontend \
            --region southamerica-east1 \
            --platform managed

          gcloud run deploy backend \
            --image gcr.io/$PROJECT_ID/backend \
            --region southamerica-east1 \
            --platform managed

      - name: Run database migrations
        run: |
          gcloud run jobs execute db-migrate \
            --region southamerica-east1
```

**Configuração**:
1. Criar Service Account no GCP
2. Baixar chave JSON
3. Adicionar em GitHub Secrets (`GCP_SA_KEY`)
4. Push na branch `main` → Deploy automático

---

## 7. SETUP INICIAL GCP (PASSO A PASSO)

### 7.1. Criar Projeto GCP

```bash
# Instalar gcloud CLI
# https://cloud.google.com/sdk/docs/install

# Login
gcloud auth login

# Criar projeto
gcloud projects create infoapp-platform --name="InfoApp Platform"

# Selecionar projeto
gcloud config set project infoapp-platform

# Ativar billing (obrigatório após free tier)
gcloud billing accounts list
gcloud billing projects link infoapp-platform --billing-account=ACCOUNT_ID
```

### 7.2. Ativar APIs Necessárias

```bash
gcloud services enable run.googleapis.com
gcloud services enable sql-component.googleapis.com
gcloud services enable sqladmin.googleapis.com
gcloud services enable storage-api.googleapis.com
gcloud services enable cloudfunctions.googleapis.com
gcloud services enable cloudbuild.googleapis.com
```

### 7.3. Criar Cloud SQL

```bash
# Criar instância PostgreSQL
gcloud sql instances create infoapp-db \
  --database-version=POSTGRES_15 \
  --tier=db-f1-micro \
  --region=southamerica-east1

# Criar banco de dados
gcloud sql databases create infoapp --instance=infoapp-db

# Criar usuário
gcloud sql users create appuser \
  --instance=infoapp-db \
  --password=SENHA_SEGURA
```

### 7.4. Deploy Backend

```bash
# Build e push imagem
cd backend
gcloud builds submit --tag gcr.io/infoapp-platform/backend

# Deploy Cloud Run
gcloud run deploy backend \
  --image gcr.io/infoapp-platform/backend \
  --region southamerica-east1 \
  --platform managed \
  --allow-unauthenticated \
  --add-cloudsql-instances infoapp-platform:southamerica-east1:infoapp-db \
  --set-env-vars DATABASE_URL=postgresql://appuser:SENHA@/infoapp?host=/cloudsql/infoapp-platform:southamerica-east1:infoapp-db
```

### 7.5. Deploy Frontend

```bash
cd frontend
gcloud builds submit --tag gcr.io/infoapp-platform/frontend

gcloud run deploy frontend \
  --image gcr.io/infoapp-platform/frontend \
  --region southamerica-east1 \
  --platform managed \
  --allow-unauthenticated \
  --set-env-vars NEXT_PUBLIC_API_URL=https://backend-xxx.a.run.app
```

---

## 8. MONITORAMENTO E ALERTAS

### 8.1. Alertas de Custo

**Configurar alerta de billing**:
```bash
# Criar alerta quando custo > $100/mês
gcloud billing budgets create \
  --billing-account=ACCOUNT_ID \
  --display-name="Alerta InfoApp $100" \
  --budget-amount=100USD
```

### 8.2. Uptime Monitoring

**Criar check de uptime**:
- Frontend: ping a cada 5 min
- Backend: ping `/health` a cada 5 min
- Alerta se down > 5 min

---

## 9. SEGURANÇA

### 9.1. Secrets Management

**Usar Secret Manager** (não .env em produção):
```bash
# Criar secret
echo -n "ELEVENLABS_API_KEY_AQUI" | gcloud secrets create elevenlabs-key --data-file=-

# Dar acesso ao Cloud Run
gcloud secrets add-iam-policy-binding elevenlabs-key \
  --member="serviceAccount:SERVICE_ACCOUNT_EMAIL" \
  --role="roles/secretmanager.secretAccessor"
```

### 9.2. IAM Permissions

**Princípio do menor privilégio**:
- Cloud Run service account: apenas SQL + Storage + Secrets
- CI/CD service account: apenas Build + Deploy
- Admin: acesso total

---

## 10. BACKUP E DISASTER RECOVERY

### 10.1. Cloud SQL Backups

**Configuração**:
- Backup automático diário (4 AM UTC-3)
- Retenção: 7 dias
- Point-in-time recovery: ativado

```bash
gcloud sql instances patch infoapp-db \
  --backup-start-time=07:00 \
  --enable-bin-log
```

### 10.2. Cloud Storage Versioning

**Ativar versionamento** (recuperar assets deletados):
```bash
gsutil versioning set on gs://infoapp-assets
```

---

## 11. RECOMENDAÇÃO FINAL

### 11.1. Para Cliente Iniciante

**Recomendamos**:
1. **v1 (MVP)**: Rodar em localhost (Docker Compose) para testar
2. **v1 (Produção)**: Deploy no GCP free tier ($0 por 3 meses)
3. **v1.1**: Migrar para modelo self-hosted (cada criador paga próprio GCP)

**Razão**: Cliente não precisa pagar conta recorrente, código é open source, criadores pagam própria infraestrutura.

### 11.2. Stack Técnica Recomendada

**Frontend**:
- Next.js 14 (React + SSR)
- TypeScript
- Tailwind CSS
- Shadcn UI (componentes)

**Backend**:
- FastAPI (Python) OU Django REST Framework
- PostgreSQL 15
- Redis (cache, filas)
- Celery (tarefas assíncronas)

**Infraestrutura**:
- Docker + Docker Compose (dev)
- GCP Cloud Run (produção)
- GitHub Actions (CI/CD)

---

**Criado por**: DevOps + Tech Lead
**Revisado por**: [Aguardando]
**Aprovado por**: [Aguardando validação do cliente]
