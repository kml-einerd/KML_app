# Resumo de Mudanças Arquiteturais v3.0

**Data**: 2025-12-27
**Responsável**: Time completo de especialistas
**Status**: CONCLUÍDO (aguardando validação cliente para perguntas adicionais)
**Versão**: 3.0 (Reformulação completa baseada em respostas do cliente)

---

## MUDANÇAS CRÍTICAS IMPLEMENTADAS

### ✅ 1. XP REMOVIDO COMPLETAMENTE → APENAS COINS

**Cliente disse**: "Apenas Coins (não existe XP) - vamos simplificar, coins já são uma boa base"

**Antes** (v2.0):
- XP: Progresso (acumula, não gasta)
- Coins: Moeda (gasta na loja)
- Sistemas independentes

**Agora** (v3.0):
- **Apenas Coins** (sistema unificado)
- Coins acumulados (lifetime) = progresso → define nível
- Coins disponíveis (saldo) = moeda → gasta na loja

**Níveis baseados em Coins lifetime**:
- Bronze: 0-100 coins
- Prata: 101-500 coins
- Ouro: 501-2.000 coins
- Diamante: 2.001-10.000 coins
- Lenda: 10.001+ coins

**Arquivo atualizado**: `/14_SISTEMA_GAMIFICACAO/economia_coins.md`
**Arquivo antigo**: `/14_SISTEMA_GAMIFICACAO/economia_xp_coins_DEPRECATED.md` (backup)

---

### ✅ 2. BILLING/MONETIZAÇÃO REMOVIDOS → OPEN SOURCE

**Cliente disse**:
- ✅ "Grátis para todos (open source, sem billing)"
- ✅ "Roda em servidor cloud (GCP)"
- ✅ "Quero conseguir visualizar e testar localmente"

**Antes** (v2.0):
- Planos pagos: Free/Starter/Pro
- Cobrança mensal: $29-$299/mês
- Comissão em vendas da loja

**Agora** (v3.0):
- **Open source** (código no GitHub)
- **Self-hosted** (cada criador hospeda próprio servidor)
- **Sem billing** (sem cobrança plataforma → criador)

**Opções de hospedagem**:
- **Localhost**: Docker Compose (desenvolvimento/testes)
- **Google Cloud**: Cloud Run + Cloud SQL (produção)
- **GitHub Actions**: CI/CD automático

**Arquivo atualizado**: `/03_SPECS_TELAS/creator_studio/10_billing.md` (marcado como DEPRECATED)
**Arquivo criado**: `/13_ARQUITETURA_PRODUTO/arquitetura_gcp.md`

---

### ✅ 3. UPLOAD EM MASSA → YAML + JSON (REMOVIDO CSV)

**Cliente disse**: "Acho que pode ser só YAML e JSON pra padronizar e ter qualidade no upload massivo"

**Antes** (v2.0):
- CSV (primário)
- JSON (secundário)

**Agora** (v3.0):
- **YAML** (primário, mais legível)
- **JSON** (secundário, compatível com ferramentas)
- **Removido CSV** (menos estruturado, difícil validar)

**Estrutura de arquivo**:
```
infoapp_pack.zip
├── manifest.yaml
├── tracks/
│   └── track_001.yaml
├── lessons/
│   └── lesson_001.yaml
├── applications/
│   └── app_001.yaml
├── srs/
│   └── vocab.yaml
└── assets/
    ├── images/
    └── audios/
```

**Arquivos criados**:
- `/18_GUIA_IMPORTACAO/estrutura_yaml.md` (guia completo)
- `/18_GUIA_IMPORTACAO/exemplos_completos/exemplo_basico.yaml` (template)
- `/18_GUIA_IMPORTACAO/boas_praticas.md` (qualidade pedagógica)

---

### ✅ 4. IA PARA CONVERTER PDF → REMOVIDA (100% MANUAL)

**Cliente disse**: "100% manual (admin escreve tudo no CSV/JSON) - é importante ter um guia para os padrões de importação"

**Antes** (v2.0):
- v1.1: IA processa PDF/ebook → gera CSV automaticamente

**Agora** (v3.0):
- **100% manual** (criador escreve YAML/JSON manualmente)
- **SEM IA** (nem v1, nem v1.1)
- **Guia detalhado** de padrões de importação

**Razão**: Cliente quer controle total sobre qualidade pedagógica

**Arquivo criado**: `/18_GUIA_IMPORTACAO/boas_praticas.md` (padrões de qualidade)

---

### ✅ 5. PRODUTOS GRÁTIS NA LOJA → POSSÍVEL MAS NÃO NOS TEMPLATES

**Cliente disse**: "Sim, exatamente isso" (confirmando que é possível, mas templates não incluem)

**Decisão**:
- ✅ É **tecnicamente possível** criar produto de 0 Coins (grátis)
- ❌ Templates prontos **NÃO incluem** produtos grátis (apenas pagos)
- **Razão**: Produtos grátis quebram economia de gamificação

**Implementação**:
```yaml
store_products:
  - id: "prod_free"
    price_coins: 0  # Permitido, mas não recomendado
```

---

## ARQUIVOS CRIADOS (NOVOS)

### 1. Perguntas Adicionais para Cliente

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md`

**Conteúdo**: 5 perguntas críticas que surgiram após respostas iniciais:
1. "Rodar no GitHub" - o que significa? (Pages/Actions/Codespaces/Repo)
2. Open source sem billing - como criador ganha dinheiro?
3. Google Cloud não é grátis - quem paga após free tier?
4. Níveis sem XP - como funcionam? (Coins lifetime/lições completadas/sem níveis)
5. Loja de recompensas - produtos pagos com Coins OU Reais?

**Status**: Aguardando resposta do cliente

---

### 2. Guias de Setup (Localhost, GCP, GitHub)

**Arquivos criados**:
- `/17_GUIA_SETUP/localhost_docker.md` (rodar localmente com Docker)
- `/17_GUIA_SETUP/deploy_gcp.md` (deploy no Google Cloud)
- `/17_GUIA_SETUP/github_cicd.md` (CI/CD com GitHub Actions)

**Público-alvo**: Criadores iniciantes (documentação MUITO clara, passo-a-passo)

**Conteúdo**:
- Pré-requisitos (instalar Docker, Git, gcloud CLI)
- Comandos completos (copy-paste)
- Troubleshooting (erros comuns)
- Screenshots/exemplos visuais

---

### 3. Guias de Importação (YAML/JSON)

**Arquivos criados**:
- `/18_GUIA_IMPORTACAO/estrutura_yaml.md` (estrutura completa de arquivos YAML)
- `/18_GUIA_IMPORTACAO/exemplos_completos/exemplo_basico.yaml` (template pronto)
- `/18_GUIA_IMPORTACAO/boas_praticas.md` (qualidade pedagógica + técnica)

**Conteúdo**:
- Estrutura de manifest, tracks, lessons, applications, SRS
- Tipos de checkpoint (QUIZ, REFLECTION, MATCH, TRUE_FALSE, FILL_BLANK)
- Boas práticas pedagógicas (beats 45-90 seg, checkpoints obrigatórios, exemplos reais)
- Validação (YAML Lint, schema JSON)

---

### 4. Arquitetura GCP

**Arquivo**: `/13_ARQUITETURA_PRODUTO/arquitetura_gcp.md`

**Conteúdo**:
- Diagrama de arquitetura (Cloud Run + Cloud SQL + Cloud Storage + Cloud Functions)
- Custo estimado (free tier: $0-25/mês, pós free tier: $20-60/mês)
- Setup passo-a-passo (criar projeto, ativar APIs, deploy)
- Monitoramento (logs, alertas de custo)

**Stack recomendada**:
- Frontend: Next.js 14 (React + SSR)
- Backend: FastAPI (Python) ou Django REST
- Database: PostgreSQL 15
- Cache: Redis
- TTS: ElevenLabs (via Cloud Functions)

---

### 5. Sistema de Gamificação (Apenas Coins)

**Arquivo**: `/14_SISTEMA_GAMIFICACAO/economia_coins.md`

**Conteúdo**:
- Sistema unificado de Coins (lifetime + saldo)
- Múltiplas formas de ganhar Coins (estilo Duolingo)
- Níveis baseados em Coins lifetime
- Loja de recompensas (personalização, power-ups, digital, desconto)
- Ligas/ranking semanal
- Streaks e daily goals

**Mudança crítica**: Removido XP completamente (apenas Coins)

---

## ARQUIVOS MODIFICADOS (ATUALIZADOS)

### 1. Billing (Marcado como DEPRECATED)

**Arquivo**: `/03_SPECS_TELAS/creator_studio/10_billing.md`

**Mudança**: Adicionado aviso de deprecação no topo:
```markdown
# BILLING (⚠️ DEPRECATED - REMOVIDO EM v3.0)

**Status**: DEPRECATED
**Razão**: Cliente decidiu por modelo open source sem billing
```

---

### 2. Perguntas Críticas (Marcadas como RESPONDIDAS)

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_criticas.md`

**Status**: Cliente respondeu perguntas 1-15
**Pendente**: Perguntas adicionais (arquivo novo criado)

---

## DÚVIDAS PENDENTES (AGUARDANDO CLIENTE)

### 🟡 Dúvida 1: "Rodar no GitHub" significa o quê?

**Opções**:
- A) GitHub Pages (landing page estática)
- B) GitHub Actions (CI/CD)
- C) GitHub Codespaces (dev cloud)
- D) Apenas repositório (versionar código)

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md` (Pergunta 1)

---

### 🟡 Dúvida 2: Como criador ganha dinheiro?

**Opções**:
- A) InfoApp 100% grátis (criador não monetiza via plataforma)
- B) Criador vende produtos externos (plataforma é funil)
- C) Loja aceita Coins OU Reais (duplo preço)
- D) Código open source mas cobra acesso

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md` (Pergunta 2)

---

### 🟡 Dúvida 3: Quem paga Google Cloud após free tier?

**Opções**:
- A) Você (cliente) paga GCP central para todos
- B) Cada criador paga próprio GCP (self-hosted)
- C) Apenas free tier (sem compromisso após)
- D) Localhost é suficiente

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md` (Pergunta 3)

---

### 🟡 Dúvida 4: Níveis sem XP - como funcionam?

**Opções**:
- A) Níveis baseados em Coins acumulados (lifetime)
- B) Não tem níveis (apenas Coins + Badges)
- C) Níveis baseados em lições completadas

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md` (Pergunta 4)

---

### 🟡 Dúvida 5: Loja aceita Coins OU Reais?

**Opções**:
- A) Apenas Coins (gamificação pura)
- B) Coins OU Reais (duplo preço)
- C) Apenas Reais (Coins desbloqueiam produtos)

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md` (Pergunta 5)

---

## RECOMENDAÇÕES TÉCNICAS (STACK)

### Frontend

**Recomendação**: Next.js 14 (React + TypeScript)

**Razão**:
- SSR/SSG (SEO-friendly)
- React (biblioteca popular, fácil contratar dev)
- TypeScript (type safety)
- Tailwind CSS (rápido, responsivo)

**Alternativa**: Create React App (mais simples, sem SSR)

---

### Backend

**Recomendação**: FastAPI (Python)

**Razão**:
- Rápido (async/await)
- Fácil de aprender (Python)
- Documentação automática (Swagger)
- Type hints (Pydantic)

**Alternativa**: Django REST Framework (mais robusto, mais complexo)

---

### Database

**Recomendação**: PostgreSQL 15

**Razão**:
- Relacional (estrutura clara)
- JSON support (flexibilidade)
- Free tier GCP (db-f1-micro)
- Backup automático

**Alternativa**: MySQL (mais comum, menos features)

---

### Infraestrutura

**Recomendação**: Google Cloud Platform (GCP)

**Razão**:
- Free tier generoso ($300 crédito + 12 meses)
- Cloud Run (serverless, auto-scale, paga por uso)
- Cloud SQL (managed PostgreSQL)
- Cloud Functions (TTS isolado)

**Alternativa**: AWS (mais complexo, mais caro)

---

## ESTIMATIVA DE CUSTO (GCP)

### Free Tier (primeiros 12 meses)

- Cloud Run: 2M requests/mês (grátis)
- Cloud SQL: db-f1-micro (grátis)
- Cloud Storage: 5 GB (grátis)
- **Total**: **~$0-25/mês** (dentro do free tier)

---

### Pós Free Tier (após 12 meses)

- Cloud Run (backend, 1 instância): ~$10-15/mês
- Cloud SQL (db-f1-micro): ~$10/mês
- Cloud Storage (50 GB assets): ~$1/mês
- ElevenLabs API (TTS): ~$5-30/mês (depende de uso)
- **Total**: **~$25-60/mês**

---

## PRÓXIMOS PASSOS

### 1. Cliente responde perguntas adicionais

**Arquivo**: `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md`

**Prazo sugerido**: 2-3 dias úteis

---

### 2. Time ajusta arquitetura final

**Baseado nas respostas**:
- Definir modelo de monetização (se houver)
- Definir quem paga hospedagem (cliente/criador/ambos)
- Definir sistema de níveis (Coins lifetime OU lições completadas)
- Atualizar documentação final

---

### 3. Criar protótipo de alta fidelidade (Figma)

**Após validação de arquitetura**:
- Designer cria mockups de todas as telas
- Cliente valida design
- Dev inicia implementação

---

### 4. Iniciar desenvolvimento v1 (Sprint 1)

**Setup**:
- Criar repositório GitHub
- Setup Docker Compose (localhost)
- Setup GCP (produção)
- CI/CD (GitHub Actions)

**Sprint 1** (2 semanas):
- Backend: Autenticação + CRUD básico
- Frontend: Login/Signup + Home
- Deploy em staging (GCP)

---

## RESUMO EXECUTIVO

### O que foi feito

✅ **XP removido** → Sistema unificado de Coins
✅ **Billing removido** → Open source + self-hosted
✅ **Upload YAML/JSON** → Guias completos criados
✅ **IA removida** → 100% manual com guias de qualidade
✅ **Arquitetura GCP** → Guias de setup (localhost, GCP, CI/CD)
✅ **Perguntas adicionais** → 5 dúvidas críticas documentadas

---

### O que falta

🟡 **Cliente responder perguntas adicionais** (5 perguntas)
🟡 **Validar stack técnica** (Next.js + FastAPI + PostgreSQL + GCP)
🟡 **Definir modelo de monetização** (se houver)
🟡 **Definir quem paga hospedagem** (cliente/criador)

---

### Arquivos criados (total: 11 novos)

1. `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md`
2. `/17_GUIA_SETUP/localhost_docker.md`
3. `/17_GUIA_SETUP/deploy_gcp.md`
4. `/17_GUIA_SETUP/github_cicd.md`
5. `/18_GUIA_IMPORTACAO/estrutura_yaml.md`
6. `/18_GUIA_IMPORTACAO/boas_praticas.md`
7. `/18_GUIA_IMPORTACAO/exemplos_completos/exemplo_basico.yaml`
8. `/13_ARQUITETURA_PRODUTO/arquitetura_gcp.md`
9. `/14_SISTEMA_GAMIFICACAO/economia_coins.md`
10. `/14_SISTEMA_GAMIFICACAO/economia_xp_coins_DEPRECATED.md` (backup)
11. `/RESUMO_MUDANCAS_ARQUITETURAIS_v3.md` (este arquivo)

---

### Arquivos modificados (total: 2)

1. `/03_SPECS_TELAS/creator_studio/10_billing.md` (marcado DEPRECATED)
2. `/16_CONFIRMACOES_CLIENTE/perguntas_criticas.md` (status atualizado)

---

## CONTATO

**Para esclarecer dúvidas**: Responder perguntas em `/16_CONFIRMACOES_CLIENTE/perguntas_adicionais.md`

**Time responsável**:
- Product Architect
- Tech Lead
- DevOps
- EdTech Specialist
- UX Architect

---

**Data de criação**: 2025-12-27
**Versão**: 3.0
**Status**: Aguardando validação do cliente
