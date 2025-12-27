# Guia de Setup: GitHub Actions (CI/CD Automático)

**Versão**: 1.0
**Data**: 2025-12-27
**Objetivo**: Configurar deploy automático (commit → teste → deploy GCP)

---

## O QUE É CI/CD?

**CI/CD** = Continuous Integration / Continuous Deployment

**O que faz**:
1. Você faz commit no GitHub (branch `main`)
2. GitHub Actions automaticamente:
   - Roda testes
   - Faz build das imagens Docker
   - Deploy no GCP (produção)

**Benefício**: Não precisa rodar comandos manuais toda vez que atualizar código.

---

## PRÉ-REQUISITOS

1. **Código no GitHub**
   - Repositório criado: `https://github.com/SEU_USUARIO/infoapp-platform`

2. **Deploy GCP funcionando**
   - Ver guia: `/17_GUIA_SETUP/deploy_gcp.md`
   - Projeto GCP: `infoapp-prod`

3. **gcloud CLI instalado**
   - Ver guia de deploy GCP

---

## PASSO 1: CRIAR SERVICE ACCOUNT (GCP)

### 1.1. Criar Service Account

```bash
# Login no GCP
gcloud auth login
gcloud config set project infoapp-prod

# Criar service account para CI/CD
gcloud iam service-accounts create github-actions \
  --display-name="GitHub Actions CI/CD"

# Pegar email do service account
SA_EMAIL=github-actions@infoapp-prod.iam.gserviceaccount.com
```

---

### 1.2. Dar Permissões

```bash
# Permissões necessárias para deploy
gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/run.admin"

gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/cloudbuild.builds.builder"

gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/iam.serviceAccountUser"

gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/storage.admin"
```

---

### 1.3. Gerar Chave JSON

```bash
# Criar chave JSON (credencial para GitHub)
gcloud iam service-accounts keys create github-sa-key.json \
  --iam-account=$SA_EMAIL

# ATENÇÃO: Arquivo github-sa-key.json foi criado
# Guarde este arquivo em local seguro
# NÃO commite no git (adicione ao .gitignore)
```

---

## PASSO 2: ADICIONAR SECRET NO GITHUB

### 2.1. Copiar Conteúdo do JSON

```bash
# Ver conteúdo do arquivo
cat github-sa-key.json

# Selecionar TODO o conteúdo e copiar (Ctrl+C)
```

---

### 2.2. Adicionar Secret no GitHub

1. Acesse: `https://github.com/SEU_USUARIO/infoapp-platform`
2. Clique em **Settings** (aba)
3. No menu lateral: **Secrets and variables** → **Actions**
4. Clique em **New repository secret**
5. Preencha:
   - **Name**: `GCP_SA_KEY`
   - **Value**: Cole o conteúdo do `github-sa-key.json`
6. Clique em **Add secret**

---

### 2.3. Adicionar Outros Secrets

**Adicione também**:

| Name | Value |
|------|-------|
| `GCP_PROJECT_ID` | `infoapp-prod` |
| `GCP_REGION` | `southamerica-east1` |
| `ELEVENLABS_API_KEY` | Sua chave ElevenLabs |
| `DATABASE_URL` | Connection string do Cloud SQL |

---

## PASSO 3: CRIAR WORKFLOW GITHUB ACTIONS

### 3.1. Criar Arquivo de Workflow

```bash
# No repositório local
mkdir -p .github/workflows

# Criar arquivo deploy.yml
touch .github/workflows/deploy.yml
```

---

### 3.2. Editar deploy.yml

**Arquivo**: `.github/workflows/deploy.yml`

```yaml
name: Deploy to GCP

on:
  push:
    branches:
      - main  # Deploy automático ao push na main

  workflow_dispatch:  # Permite executar manualmente

jobs:
  test:
    name: Run Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          cd backend
          pip install -r requirements.txt

      - name: Run tests
        run: |
          cd backend
          pytest tests/ -v

  build-and-deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest
    needs: test  # Só deploy se testes passarem

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@v1
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}

      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v1

      - name: Configure Docker for GCR
        run: |
          gcloud auth configure-docker

      - name: Build and Push Backend
        run: |
          cd backend
          gcloud builds submit --tag gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend

      - name: Build and Push Frontend
        run: |
          cd frontend
          gcloud builds submit --tag gcr.io/${{ secrets.GCP_PROJECT_ID }}/frontend

      - name: Deploy Backend to Cloud Run
        run: |
          gcloud run deploy backend \
            --image gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend \
            --region ${{ secrets.GCP_REGION }} \
            --platform managed \
            --allow-unauthenticated

      - name: Deploy Frontend to Cloud Run
        run: |
          gcloud run deploy frontend \
            --image gcr.io/${{ secrets.GCP_PROJECT_ID }}/frontend \
            --region ${{ secrets.GCP_REGION }} \
            --platform managed \
            --allow-unauthenticated

      - name: Run Database Migrations
        run: |
          gcloud run jobs execute db-migrate \
            --region ${{ secrets.GCP_REGION }} \
            --wait

  notify:
    name: Notify Deployment
    runs-on: ubuntu-latest
    needs: build-and-deploy
    if: always()

    steps:
      - name: Notify Success
        if: success()
        run: echo "✅ Deploy concluído com sucesso!"

      - name: Notify Failure
        if: failure()
        run: echo "❌ Deploy falhou. Verifique logs."
```

---

### 3.3. Commit e Push

```bash
# Adicionar arquivo
git add .github/workflows/deploy.yml

# Commit
git commit -m "Add GitHub Actions CI/CD workflow"

# Push (vai triggar deploy automático!)
git push origin main
```

---

## PASSO 4: VERIFICAR EXECUÇÃO

### 4.1. Ver Workflow no GitHub

1. Acesse: `https://github.com/SEU_USUARIO/infoapp-platform`
2. Clique na aba **Actions**
3. Você verá workflow **Deploy to GCP** em execução

---

### 4.2. Acompanhar Logs

1. Clique no workflow em execução
2. Expanda cada job:
   - **test**: Rodar testes
   - **build-and-deploy**: Build + Deploy
   - **notify**: Notificação final

**Tempo estimado**: 10-15 min (primeira vez), 5-8 min (subsequentes)

---

### 4.3. Verificar Deploy

```bash
# Verificar se nova versão foi deployada
gcloud run services describe backend \
  --region southamerica-east1 \
  --format='value(status.latestCreatedRevisionName)'

# Saída exemplo: backend-00012-abc (número incrementa a cada deploy)
```

---

## PASSO 5: WORKFLOW AVANÇADO (STAGING + PRODUÇÃO)

### 5.1. Criar Ambiente de Staging

**Objetivo**: Testar mudanças em staging antes de ir pra produção

**Estratégia**:
- Branch `develop` → Deploy automático para **staging**
- Branch `main` → Deploy automático para **produção**

---

### 5.2. Atualizar Workflow

```yaml
name: Deploy to GCP

on:
  push:
    branches:
      - main      # Produção
      - develop   # Staging

jobs:
  test:
    # ... mesmo código ...

  build-and-deploy:
    runs-on: ubuntu-latest
    needs: test

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set environment
        id: set-env
        run: |
          if [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
            echo "ENVIRONMENT=production" >> $GITHUB_OUTPUT
            echo "GCP_PROJECT=infoapp-prod" >> $GITHUB_OUTPUT
          else
            echo "ENVIRONMENT=staging" >> $GITHUB_OUTPUT
            echo "GCP_PROJECT=infoapp-staging" >> $GITHUB_OUTPUT
          fi

      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@v1
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}

      - name: Deploy Backend
        run: |
          gcloud builds submit --tag gcr.io/${{ steps.set-env.outputs.GCP_PROJECT }}/backend backend/
          gcloud run deploy backend \
            --image gcr.io/${{ steps.set-env.outputs.GCP_PROJECT }}/backend \
            --region southamerica-east1 \
            --project ${{ steps.set-env.outputs.GCP_PROJECT }}

      # ... mesmo para frontend ...
```

---

## PASSO 6: ADICIONAR TESTES AUTOMÁTICOS

### 6.1. Configurar Pytest (Backend)

**Arquivo**: `backend/tests/test_health.py`

```python
def test_health_endpoint(client):
    response = client.get("/health")
    assert response.status_code == 200
    assert response.json() == {"status": "ok"}
```

**Arquivo**: `backend/pytest.ini`

```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
```

---

### 6.2. Configurar Jest (Frontend)

**Arquivo**: `frontend/jest.config.js`

```javascript
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  testPathIgnorePatterns: ['/node_modules/', '/.next/'],
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
  ],
};
```

**Atualizar workflow**:

```yaml
- name: Run Frontend Tests
  run: |
    cd frontend
    npm install
    npm test
```

---

## PASSO 7: NOTIFICAÇÕES (SLACK/DISCORD)

### 7.1. Adicionar Notificação Slack (Opcional)

**Pré-requisito**: Webhook do Slack

**Adicionar step**:

```yaml
- name: Notify Slack
  if: always()
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    text: 'Deploy para ${{ steps.set-env.outputs.ENVIRONMENT }} finalizado!'
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

**Configurar Secret**:
- Name: `SLACK_WEBHOOK`
- Value: URL do webhook do Slack

---

## TROUBLESHOOTING

### Erro: "Authentication failed"

**Causa**: Secret `GCP_SA_KEY` está incorreto

**Solução**:
1. Regerar chave JSON:
   ```bash
   gcloud iam service-accounts keys create github-sa-key-new.json \
     --iam-account=github-actions@infoapp-prod.iam.gserviceaccount.com
   ```
2. Atualizar secret no GitHub

---

### Erro: "Permission denied to deploy"

**Causa**: Service account não tem permissão

**Solução**:
```bash
gcloud projects add-iam-policy-binding infoapp-prod \
  --member="serviceAccount:github-actions@infoapp-prod.iam.gserviceaccount.com" \
  --role="roles/run.admin"
```

---

### Workflow não executa

**Causa**: Arquivo YAML está em local errado

**Solução**:
- Verificar caminho: `.github/workflows/deploy.yml`
- Verificar sintaxe YAML (sem tabs, apenas espaços)

---

## COMANDOS ÚTEIS

### Executar workflow manualmente

1. Acesse: **Actions** → **Deploy to GCP**
2. Clique em **Run workflow**
3. Selecione branch (`main` ou `develop`)
4. Clique em **Run workflow**

---

### Ver logs de deploy anterior

```bash
# Listar execuções
gh run list --workflow=deploy.yml

# Ver logs de uma execução
gh run view RUN_ID --log
```

---

## PRÓXIMOS PASSOS

1. **Configurar ambiente de staging**: Criar projeto `infoapp-staging` no GCP
2. **Adicionar mais testes**: Aumentar cobertura de testes (> 80%)
3. **Rollback automático**: Se deploy falhar, voltar para versão anterior

---

**Criado por**: DevOps + CI/CD Specialist
**Revisado por**: [Aguardando feedback]
