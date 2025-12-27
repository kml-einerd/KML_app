# Guia de Setup: Deploy no Google Cloud Platform (GCP)

**Versão**: 1.0
**Data**: 2025-12-27
**Público**: Criadores com conhecimento técnico intermediário
**Objetivo**: Deploy da InfoApp Platform no GCP (produção)

---

## PRÉ-REQUISITOS

1. **Conta Google Cloud**
   - Acesse: https://console.cloud.google.com/
   - Crie conta (free tier: $300 crédito + 12 meses grátis)
   - Adicione cartão de crédito (obrigatório, mas não será cobrado no free tier)

2. **gcloud CLI instalado**
   - Guia: https://cloud.google.com/sdk/docs/install
   - Verificar: `gcloud --version`

3. **Docker instalado**
   - Ver guia: `/17_GUIA_SETUP/localhost_docker.md`

---

## PASSO 1: CONFIGURAR PROJETO GCP

### 1.1. Criar Projeto

```bash
# Login no GCP
gcloud auth login

# Criar projeto
gcloud projects create infoapp-prod --name="InfoApp Production"

# Listar projetos
gcloud projects list

# Selecionar projeto
gcloud config set project infoapp-prod
```

---

### 1.2. Ativar Billing

**Via Console Web**:
1. Acesse: https://console.cloud.google.com/billing
2. Selecione projeto `infoapp-prod`
3. Link com conta de billing

**Via CLI**:
```bash
# Listar contas de billing
gcloud billing accounts list

# Associar projeto à conta
gcloud billing projects link infoapp-prod \
  --billing-account=0X0X0X-0X0X0X-0X0X0X
```

---

### 1.3. Ativar APIs

```bash
# Ativar todas as APIs necessárias
gcloud services enable run.googleapis.com \
  sql-component.googleapis.com \
  sqladmin.googleapis.com \
  storage-api.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  secretmanager.googleapis.com \
  compute.googleapis.com
```

---

## PASSO 2: CRIAR CLOUD SQL (BANCO DE DADOS)

### 2.1. Criar Instância PostgreSQL

```bash
# Criar instância (free tier)
gcloud sql instances create infoapp-db \
  --database-version=POSTGRES_15 \
  --tier=db-f1-micro \
  --region=southamerica-east1 \
  --no-backup \
  --no-assign-ip

# Aguardar ~5-10 minutos
```

**Opções**:
- `db-f1-micro`: Free tier (614 MB RAM, 1 vCPU compartilhada)
- `southamerica-east1`: São Paulo (latência menor no Brasil)
- `--no-assign-ip`: Apenas conexão privada (mais seguro)

---

### 2.2. Criar Banco de Dados

```bash
# Criar database
gcloud sql databases create infoapp \
  --instance=infoapp-db

# Criar usuário
gcloud sql users create appuser \
  --instance=infoapp-db \
  --password=SENHA_SEGURA_AQUI

# Anotar senha (você vai precisar depois)
```

---

### 2.3. Obter Connection Name

```bash
# Pegar connection string
gcloud sql instances describe infoapp-db \
  --format='value(connectionName)'

# Saída exemplo:
# infoapp-prod:southamerica-east1:infoapp-db

# ANOTAR ESTE VALOR (vai usar no deploy)
```

---

## PASSO 3: CRIAR SECRETS (CHAVES DE API)

### 3.1. Criar Secret para ElevenLabs

```bash
# Criar secret
echo -n "SUA_CHAVE_ELEVENLABS_AQUI" | \
  gcloud secrets create elevenlabs-api-key \
  --data-file=-

# Verificar
gcloud secrets list
```

---

### 3.2. Criar Secret para Django SECRET_KEY

```bash
# Gerar chave segura
python3 -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'

# Criar secret
echo -n "CHAVE_GERADA_ACIMA" | \
  gcloud secrets create django-secret-key \
  --data-file=-
```

---

## PASSO 4: CRIAR CLOUD STORAGE (ASSETS)

### 4.1. Criar Bucket

```bash
# Criar bucket para assets
gsutil mb -c STANDARD -l southamerica-east1 gs://infoapp-assets

# Tornar bucket público (para servir imagens/áudios)
gsutil iam ch allUsers:objectViewer gs://infoapp-assets

# Ativar versionamento (backup)
gsutil versioning set on gs://infoapp-assets
```

---

## PASSO 5: BUILD E PUSH DE IMAGENS DOCKER

### 5.1. Build Backend

```bash
# Navegar para pasta do projeto
cd ~/projetos/infoapp-platform

# Build e push backend
cd backend
gcloud builds submit --tag gcr.io/infoapp-prod/backend

# Aguardar ~5-10 min (primeira vez)
```

---

### 5.2. Build Frontend

```bash
cd ../frontend
gcloud builds submit --tag gcr.io/infoapp-prod/frontend
```

---

## PASSO 6: DEPLOY BACKEND (CLOUD RUN)

### 6.1. Deploy com Configurações

```bash
# Deploy backend
gcloud run deploy backend \
  --image gcr.io/infoapp-prod/backend \
  --region southamerica-east1 \
  --platform managed \
  --allow-unauthenticated \
  --min-instances 1 \
  --max-instances 10 \
  --memory 1Gi \
  --cpu 2 \
  --timeout 300 \
  --add-cloudsql-instances infoapp-prod:southamerica-east1:infoapp-db \
  --set-env-vars "DATABASE_URL=postgresql://appuser:SENHA@/infoapp?host=/cloudsql/infoapp-prod:southamerica-east1:infoapp-db" \
  --set-secrets "ELEVENLABS_API_KEY=elevenlabs-api-key:latest,SECRET_KEY=django-secret-key:latest"

# Aguardar ~2-3 min
```

**Saída esperada**:
```
Service [backend] revision [backend-00001-xxx] has been deployed and is serving 100 percent of traffic.
Service URL: https://backend-xxx-abc.a.run.app
```

**ANOTAR URL do backend** (você vai usar no frontend)

---

### 6.2. Rodar Migrations

```bash
# Pegar URL do backend
BACKEND_URL=$(gcloud run services describe backend \
  --region southamerica-east1 \
  --format='value(status.url)')

# Rodar migrations via Job
gcloud run jobs create db-migrate \
  --image gcr.io/infoapp-prod/backend \
  --region southamerica-east1 \
  --set-cloudsql-instances infoapp-prod:southamerica-east1:infoapp-db \
  --set-env-vars "DATABASE_URL=postgresql://appuser:SENHA@/infoapp?host=/cloudsql/infoapp-prod:southamerica-east1:infoapp-db" \
  --command python \
  --args "manage.py,migrate"

# Executar job
gcloud run jobs execute db-migrate \
  --region southamerica-east1
```

---

## PASSO 7: DEPLOY FRONTEND (CLOUD RUN)

### 7.1. Deploy com URL do Backend

```bash
# Deploy frontend
gcloud run deploy frontend \
  --image gcr.io/infoapp-prod/frontend \
  --region southamerica-east1 \
  --platform managed \
  --allow-unauthenticated \
  --min-instances 0 \
  --max-instances 10 \
  --memory 512Mi \
  --cpu 1 \
  --set-env-vars "NEXT_PUBLIC_API_URL=https://backend-xxx-abc.a.run.app"

# Substituir backend-xxx-abc.a.run.app pela URL anotada no Passo 6.1
```

**Saída esperada**:
```
Service URL: https://frontend-yyy-def.a.run.app
```

---

## PASSO 8: TESTAR DEPLOY

### 8.1. Acessar Frontend

1. Abra navegador
2. Acesse URL do frontend: `https://frontend-yyy-def.a.run.app`
3. Deve carregar tela de Login/Signup

---

### 8.2. Testar API

```bash
# Health check do backend
curl https://backend-xxx-abc.a.run.app/health

# Deve retornar:
# {"status": "ok"}
```

---

### 8.3. Criar Superusuário

```bash
# Rodar comando no Cloud Run (via job)
gcloud run jobs create create-superuser \
  --image gcr.io/infoapp-prod/backend \
  --region southamerica-east1 \
  --set-cloudsql-instances infoapp-prod:southamerica-east1:infoapp-db \
  --set-env-vars "DATABASE_URL=postgresql://..." \
  --command python \
  --args "manage.py,createsuperuser,--noinput,--username=admin,--email=admin@example.com"

# Executar
gcloud run jobs execute create-superuser --region southamerica-east1

# Resetar senha do admin via console depois
```

---

## PASSO 9: CONFIGURAR DOMÍNIO CUSTOMIZADO (OPCIONAL)

### 9.1. Mapear Domínio

**Pré-requisito**: Você possui domínio (ex: `meuinfoapp.com`)

```bash
# Mapear domínio ao Cloud Run
gcloud run domain-mappings create \
  --service frontend \
  --domain meuinfoapp.com \
  --region southamerica-east1
```

**Saída**:
```
Adicione estes registros DNS no seu provedor:
Type: A
Name: meuinfoapp.com
Value: 216.239.32.21
```

---

### 9.2. Configurar DNS

1. Acesse painel do seu provedor de domínio (Registro.br, GoDaddy, etc.)
2. Adicione registros DNS conforme indicado
3. Aguarde propagação (15 min - 48h)
4. Acesse: `https://meuinfoapp.com`

---

## PASSO 10: MONITORAMENTO E ALERTAS

### 10.1. Configurar Alerta de Custo

```bash
# Criar orçamento de $100/mês
gcloud billing budgets create \
  --billing-account=0X0X0X-0X0X0X-0X0X0X \
  --display-name="InfoApp Budget $100" \
  --budget-amount=100USD \
  --threshold-rule=percent=50 \
  --threshold-rule=percent=90 \
  --threshold-rule=percent=100
```

Você receberá email quando atingir 50%, 90% e 100% do orçamento.

---

### 10.2. Visualizar Logs

**Via Console Web**:
1. Acesse: https://console.cloud.google.com/logs
2. Selecione projeto: `infoapp-prod`
3. Filtros úteis:
   - Resource: Cloud Run Revision
   - Severity: Error

**Via CLI**:
```bash
# Logs do backend (últimos 10 min)
gcloud logging read "resource.type=cloud_run_revision AND resource.labels.service_name=backend" \
  --limit 50 \
  --format json

# Logs de erros
gcloud logging read "severity=ERROR" --limit 50
```

---

## PASSO 11: ATUALIZAR DEPLOY (NOVO CÓDIGO)

### 11.1. Rebuild e Redeploy

```bash
# Fazer mudanças no código local
# Commit no git
git add .
git commit -m "Update feature X"
git push origin main

# Rebuild backend
cd backend
gcloud builds submit --tag gcr.io/infoapp-prod/backend

# Redeploy (usa mesma config anterior)
gcloud run deploy backend \
  --image gcr.io/infoapp-prod/backend \
  --region southamerica-east1

# Rebuild frontend
cd ../frontend
gcloud builds submit --tag gcr.io/infoapp-prod/frontend

gcloud run deploy frontend \
  --image gcr.io/infoapp-prod/frontend \
  --region southamerica-east1
```

---

## CUSTO ESTIMADO

### Free Tier (primeiros 12 meses)

**Incluído**:
- Cloud Run: 2 milhões requests/mês
- Cloud SQL: db-f1-micro (1 instância)
- Cloud Storage: 5 GB
- Cloud Build: 120 build-minutes/dia
- **Total**: ~$0/mês (dentro do free tier)

**Após free tier**:
- Cloud Run (backend, 1 instância sempre ligada): ~$10-15/mês
- Cloud SQL (db-f1-micro): ~$10/mês
- Cloud Storage (50 GB assets): ~$1/mês
- **Total**: ~$20-25/mês

---

## TROUBLESHOOTING

### Erro: "Permission denied"

**Solução**: Dar permissões ao service account

```bash
# Pegar email do service account
SA_EMAIL=$(gcloud iam service-accounts list \
  --filter="displayName:Compute Engine default service account" \
  --format="value(email)")

# Dar permissões
gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/cloudsql.client"

gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/secretmanager.secretAccessor"
```

---

### Erro: "Cloud SQL connection failed"

**Solução**: Verificar connection string

```bash
# Verificar connection name
gcloud sql instances describe infoapp-db \
  --format='value(connectionName)'

# Deve ser exatamente: infoapp-prod:southamerica-east1:infoapp-db
```

---

### Frontend não carrega (404)

**Solução**: Verificar build do Next.js

```bash
# Reconstruir com logs
cd frontend
gcloud builds submit --tag gcr.io/infoapp-prod/frontend

# Ver logs do Cloud Run
gcloud run services logs read frontend --region southamerica-east1
```

---

## PRÓXIMOS PASSOS

1. **Configurar CI/CD**: Ver guia `/17_GUIA_SETUP/github_cicd.md` (deploy automático)
2. **Importar conteúdo**: Ver guia `/18_GUIA_IMPORTACAO/estrutura_yaml.md`
3. **Escalar**: Quando ultrapassar free tier, upgrade para db-g1-small

---

**Criado por**: DevOps + Cloud Architect
**Revisado por**: [Aguardando feedback]
