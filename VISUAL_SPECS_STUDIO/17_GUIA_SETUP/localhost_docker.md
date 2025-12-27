# Guia de Setup: Localhost com Docker

**Versão**: 1.0
**Data**: 2025-12-27
**Público**: Criadores iniciantes (sem conhecimento técnico avançado)
**Objetivo**: Rodar InfoApp Platform localmente para desenvolvimento e testes

---

## PRÉ-REQUISITOS

Antes de começar, você precisa instalar:

### 1. Docker Desktop

**Windows/Mac**:
1. Acesse: https://www.docker.com/products/docker-desktop
2. Baixe Docker Desktop
3. Instale e reinicie o computador
4. Abra Docker Desktop (ícone de baleia azul)

**Linux**:
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker

# Adicionar seu usuário ao grupo docker (para não precisar de sudo)
sudo usermod -aG docker $USER
# Faça logout e login novamente
```

**Verificar instalação**:
```bash
docker --version
# Deve retornar: Docker version 24.x.x

docker-compose --version
# Deve retornar: Docker Compose version v2.x.x
```

---

### 2. Git

**Windows**:
1. Acesse: https://git-scm.com/download/win
2. Baixe e instale

**Mac**:
```bash
# Instalar Homebrew (se não tiver)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instalar Git
brew install git
```

**Linux**:
```bash
sudo apt install git
```

**Verificar instalação**:
```bash
git --version
# Deve retornar: git version 2.x.x
```

---

### 3. Editor de Código (Opcional mas recomendado)

**Visual Studio Code**:
1. Acesse: https://code.visualstudio.com/
2. Baixe e instale
3. Instale extensões recomendadas:
   - Docker
   - Python
   - ESLint
   - Prettier

---

## PASSO 1: CLONAR REPOSITÓRIO

### 1.1. Abrir Terminal

**Windows**:
- Pressione `Windows + R`
- Digite `cmd` ou `powershell`
- Pressione Enter

**Mac**:
- Pressione `Cmd + Espaço`
- Digite `Terminal`
- Pressione Enter

**Linux**:
- Pressione `Ctrl + Alt + T`

---

### 1.2. Clonar Código do GitHub

```bash
# Navegar para pasta de projetos (crie se não existir)
cd ~
mkdir projetos
cd projetos

# Clonar repositório
git clone https://github.com/SEU_USUARIO/infoapp-platform.git

# Entrar na pasta do projeto
cd infoapp-platform

# Verificar estrutura
ls -la
# Deve mostrar: frontend/, backend/, docker-compose.yml, README.md, etc.
```

---

## PASSO 2: CONFIGURAR VARIÁVEIS DE AMBIENTE

### 2.1. Copiar Arquivo de Exemplo

```bash
# Copiar arquivo .env.example para .env
cp .env.example .env
```

---

### 2.2. Editar Arquivo .env

**Abrir .env com editor**:

**Windows (Bloco de Notas)**:
```bash
notepad .env
```

**Mac/Linux (nano)**:
```bash
nano .env
```

**OU abrir com VS Code**:
```bash
code .env
```

---

### 2.3. Configurar Chaves de API

**Conteúdo do .env**:
```bash
# ===== GERAL =====
ENVIRONMENT=development
DEBUG=true

# ===== BANCO DE DADOS =====
DATABASE_URL=postgresql://postgres:postgres@db:5432/infoapp
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=infoapp

# ===== ELEVENLABS (TTS) =====
ELEVENLABS_API_KEY=seu_api_key_aqui

# ===== FRONTEND =====
NEXT_PUBLIC_API_URL=http://localhost:8000

# ===== BACKEND =====
SECRET_KEY=chave_secreta_desenvolvimento_12345
ALLOWED_HOSTS=localhost,127.0.0.1

# ===== REDIS (CACHE) =====
REDIS_URL=redis://redis:6379/0
```

---

### 2.4. Obter API Key do ElevenLabs (TTS)

**IMPORTANTE**: Você precisa de uma conta ElevenLabs para gerar áudio (TTS).

**Passos**:
1. Acesse: https://elevenlabs.io/
2. Crie conta gratuita (10.000 caracteres/mês grátis)
3. Vá em **Profile** → **API Keys**
4. Copie sua API Key
5. Cole no arquivo `.env` no campo `ELEVENLABS_API_KEY`

**Exemplo**:
```bash
ELEVENLABS_API_KEY=sk_abc123def456ghi789jkl012mno345pqr678stu
```

**Salvar e fechar** o arquivo `.env`.

---

## PASSO 3: SUBIR CONTAINERS DOCKER

### 3.1. Iniciar Docker Desktop

**Windows/Mac**:
- Abra Docker Desktop (ícone de baleia azul)
- Aguarde até ver "Docker Desktop is running"

---

### 3.2. Subir Todos os Serviços

```bash
# No terminal, na pasta do projeto
docker-compose up -d

# O que isso faz:
# - Baixa imagens Docker (primeira vez demora ~5-10 min)
# - Cria containers: frontend, backend, db (PostgreSQL), redis
# - Inicia todos os serviços
```

**Saída esperada**:
```
Creating network "infoapp-platform_default" with the default driver
Creating volume "infoapp-platform_postgres_data" with default driver
Creating infoapp-platform_db_1      ... done
Creating infoapp-platform_redis_1   ... done
Creating infoapp-platform_backend_1 ... done
Creating infoapp-platform_frontend_1 ... done
```

---

### 3.3. Verificar Status dos Containers

```bash
docker-compose ps
```

**Saída esperada**:
```
NAME                        STATUS              PORTS
infoapp-platform_db_1       Up 2 minutes        5432/tcp
infoapp-platform_redis_1    Up 2 minutes        6379/tcp
infoapp-platform_backend_1  Up 2 minutes        0.0.0.0:8000->8000/tcp
infoapp-platform_frontend_1 Up 2 minutes        0.0.0.0:3000->3000/tcp
```

**Todos devem estar com STATUS "Up"**.

---

### 3.4. Verificar Logs (se houver erro)

```bash
# Ver logs de todos os containers
docker-compose logs

# Ver logs de um container específico
docker-compose logs backend
docker-compose logs frontend
```

---

## PASSO 4: RODAR MIGRATIONS (BANCO DE DADOS)

### 4.1. Criar Tabelas no Banco

```bash
# Executar migrations dentro do container backend
docker-compose exec backend python manage.py migrate

# Saída esperada:
# Running migrations:
#   Applying contenttypes.0001_initial... OK
#   Applying auth.0001_initial... OK
#   ...
```

---

### 4.2. Criar Usuário Admin (Opcional)

```bash
# Criar superusuário para acessar admin
docker-compose exec backend python manage.py createsuperuser

# Preencher:
# Username: admin
# Email: admin@localhost.com
# Password: admin123
```

---

## PASSO 5: ACESSAR APLICAÇÃO

### 5.1. Frontend (Learner App)

**Abrir navegador**:
- URL: http://localhost:3000

**O que você verá**:
- Tela de Login/Signup
- Após criar conta: Home com "Missão do Dia"

---

### 5.2. Backend (API Docs)

**Abrir navegador**:
- URL: http://localhost:8000/docs

**O que você verá**:
- Documentação interativa da API (Swagger)
- Pode testar endpoints diretamente no navegador

---

### 5.3. Admin Panel (Django Admin)

**Abrir navegador**:
- URL: http://localhost:8000/admin

**Login**:
- Username: `admin`
- Password: `admin123` (que você criou no Passo 4.2)

**O que você verá**:
- Interface administrativa do Django
- Pode criar/editar usuários, InfoApps, lessons, etc.

---

## PASSO 6: TESTAR FUNCIONALIDADES

### 6.1. Criar Conta de Aluno

1. Acesse: http://localhost:3000
2. Clique em **Criar Conta**
3. Preencha:
   - Nome: João Silva
   - Email: joao@test.com
   - Senha: senha123
4. Clique em **Cadastrar**
5. Faça login

---

### 6.2. Criar InfoApp (Creator Studio)

**IMPORTANTE**: Ainda não temos UI para Creator Studio em localhost. Por enquanto, use Django Admin.

1. Acesse: http://localhost:8000/admin
2. Login com `admin` / `admin123`
3. Clique em **InfoApps** → **Add InfoApp**
4. Preencha:
   - Name: Meu Primeiro InfoApp
   - Slug: meu-primeiro-infoapp
   - Language: pt-BR
5. Salvar

---

### 6.3. Criar Lesson (Aula Interativa)

1. No Django Admin, clique em **Lessons** → **Add Lesson**
2. Preencha:
   - Title: Introdução
   - InfoApp: Meu Primeiro InfoApp
   - Order: 1
3. Salvar
4. Adicionar Beats (beats da aula):
   - Beat 1: "Bem-vindo ao InfoApp!"
   - Beat 2: "Vamos aprender algo novo."
   - Checkpoint: Quiz com pergunta teste

---

### 6.4. Testar TTS (Áudio)

1. No Django Admin, edite um Beat
2. Clique em **Generate Audio** (botão de ação)
3. Aguarde (~5-10 seg)
4. Áudio será gerado via ElevenLabs
5. Play do áudio no Learner App

**Se der erro**: Verifique se `ELEVENLABS_API_KEY` está correto no `.env`.

---

## PASSO 7: PARAR E REINICIAR CONTAINERS

### 7.1. Parar Todos os Containers

```bash
# Parar containers (sem deletar volumes/dados)
docker-compose stop

# Parar e deletar containers (mantém volumes)
docker-compose down

# Parar, deletar containers E volumes (apaga banco de dados)
docker-compose down -v
```

---

### 7.2. Reiniciar Containers

```bash
# Iniciar containers novamente
docker-compose up -d

# Ver logs em tempo real (Ctrl+C para sair)
docker-compose logs -f
```

---

## PASSO 8: ATUALIZAR CÓDIGO (PULL NOVO CÓDIGO DO GITHUB)

### 8.1. Baixar Atualizações

```bash
# Parar containers
docker-compose down

# Baixar novo código
git pull origin main

# Reconstruir imagens (se houve mudança em Dockerfile)
docker-compose build

# Subir containers novamente
docker-compose up -d

# Rodar migrations (se houver novas)
docker-compose exec backend python manage.py migrate
```

---

## TROUBLESHOOTING (RESOLUÇÃO DE PROBLEMAS)

### Problema 1: "Cannot connect to Docker daemon"

**Causa**: Docker Desktop não está rodando

**Solução**:
1. Abra Docker Desktop
2. Aguarde "Docker Desktop is running"
3. Tente novamente: `docker-compose up -d`

---

### Problema 2: "Port 3000 is already in use"

**Causa**: Outro aplicativo está usando porta 3000

**Solução**:
```bash
# Encontrar processo usando porta 3000
# Windows
netstat -ano | findstr :3000

# Mac/Linux
lsof -i :3000

# Matar processo (substituir PID)
# Windows
taskkill /PID 12345 /F

# Mac/Linux
kill -9 12345
```

**OU mudar porta no docker-compose.yml**:
```yaml
frontend:
  ports:
    - "3001:3000"  # Mudar de 3000 para 3001
```

---

### Problema 3: "ELEVENLABS_API_KEY is invalid"

**Causa**: API Key do ElevenLabs está errada ou expirada

**Solução**:
1. Acesse: https://elevenlabs.io/
2. Vá em **Profile** → **API Keys**
3. Gere nova API Key
4. Atualize `.env`:
   ```bash
   ELEVENLABS_API_KEY=nova_chave_aqui
   ```
5. Reinicie containers:
   ```bash
   docker-compose restart backend
   ```

---

### Problema 4: "Database connection failed"

**Causa**: PostgreSQL não iniciou ou credenciais erradas

**Solução**:
```bash
# Verificar logs do banco
docker-compose logs db

# Recriar banco (CUIDADO: apaga dados)
docker-compose down -v
docker-compose up -d
docker-compose exec backend python manage.py migrate
```

---

### Problema 5: Containers iniciam mas site não abre

**Causa**: Backend pode estar crashando

**Solução**:
```bash
# Ver logs do backend
docker-compose logs backend

# Verificar se backend está respondendo
curl http://localhost:8000/health
# Deve retornar: {"status": "ok"}

# Se não responder, reconstruir imagem
docker-compose build backend
docker-compose up -d
```

---

## COMANDOS ÚTEIS

### Ver logs em tempo real
```bash
docker-compose logs -f
```

### Entrar dentro de um container (debug)
```bash
# Backend (Python)
docker-compose exec backend bash
# Agora você está "dentro" do container

# Frontend (Node.js)
docker-compose exec frontend sh
```

### Rodar comando dentro do container
```bash
# Criar migration
docker-compose exec backend python manage.py makemigrations

# Rodar shell Python
docker-compose exec backend python manage.py shell

# Rodar testes
docker-compose exec backend pytest
```

### Limpar tudo (recomeçar do zero)
```bash
# CUIDADO: Apaga TODOS os dados
docker-compose down -v
docker system prune -a --volumes
docker-compose up -d
```

---

## PRÓXIMOS PASSOS

Após testar localmente, você pode:

1. **Deploy no GCP**: Ver guia `/17_GUIA_SETUP/deploy_gcp.md`
2. **Configurar CI/CD**: Ver guia `/17_GUIA_SETUP/github_cicd.md`
3. **Importar conteúdo em massa**: Ver guia `/18_GUIA_IMPORTACAO/estrutura_yaml.md`

---

## RECURSOS ADICIONAIS

- **Documentação Docker**: https://docs.docker.com/
- **Documentação Docker Compose**: https://docs.docker.com/compose/
- **Troubleshooting Docker**: https://docs.docker.com/engine/faq/

---

**Criado por**: DevOps + Tech Writer
**Público-alvo**: Criadores iniciantes (não-técnicos)
**Revisado por**: [Aguardando feedback de usuários]
