# Arquitetura Técnica Multi-Tenant

**Versão**: 2.0
**Data**: 2025-12-26
**Status**: PROPOSTA (aguardando validação)
**Decisor**: Tech Lead

---

## 1. PROBLEMA

O produto é um **gerador de EdTech SaaS**. Cada criador pode criar múltiplos InfoApps, e cada InfoApp é um SaaS completo e isolado.

**Questão crítica**: Como estruturar o banco de dados e infraestrutura para suportar múltiplos InfoApps isolados?

### Requisitos técnicos:
1. **Isolamento**: Dados de cada InfoApp devem ser isolados (alunos, lessons, progresso)
2. **Escalabilidade**: Suportar 1.000+ InfoApps criados
3. **Performance**: Queries rápidas mesmo com milhares de InfoApps
4. **Backup/Restore**: Facilidade de backup por InfoApp
5. **Compliance**: LGPD/GDPR (direito ao esquecimento, portabilidade)
6. **Analytics cross-app**: Possibilidade de analytics agregados (opcional)
7. **Simplicidade**: Facilidade de desenvolvimento e manutenção

---

## 2. OPÇÕES DE ARQUITETURA MULTI-TENANT

### Opção A: Shared Database com tenant_id
### Opção B: Database isolado por InfoApp
### Opção C: Arquitetura Híbrida

---

## 3. OPÇÃO A: SHARED DATABASE COM TENANT_ID

### 3.1. Estrutura

```
┌────────────────────────────────────────────┐
│         PostgreSQL Database (único)        │
│                                            │
│  users                                     │
│  ├── id                                    │
│  ├── email                                 │
│  ├── infoapp_id  ← tenant_id               │
│  └── ...                                   │
│                                            │
│  lessons                                   │
│  ├── id                                    │
│  ├── title                                 │
│  ├── infoapp_id  ← tenant_id               │
│  └── ...                                   │
│                                            │
│  infoapps                                  │
│  ├── id                                    │
│  ├── name                                  │
│  ├── creator_id                            │
│  ├── subdomain                             │
│  └── ...                                   │
└────────────────────────────────────────────┘
```

### 3.2. Funcionamento

**Todas as tabelas têm coluna `infoapp_id`** (exceto tabelas globais como `infoapps`, `creators`)

**Query sempre filtra por tenant_id**:
```sql
-- ❌ PERIGOSO (sem tenant_id)
SELECT * FROM users WHERE email = 'user@example.com';

-- ✅ SEGURO (com tenant_id)
SELECT * FROM users
WHERE email = 'user@example.com'
  AND infoapp_id = 'abc123';
```

### 3.3. Implementação técnica

**Middleware/ORM automatiza filtro**:
```python
# Django com django-tenant-schemas ou django-tenants
# Automático: todas as queries filtram por tenant_id

# Request entra em "aprenda-python.plataforma.com"
# Middleware identifica: infoapp_id = "abc123"
# Todas as queries automaticamente adicionam WHERE infoapp_id = 'abc123'
```

**Row-Level Security (PostgreSQL)**:
```sql
-- RLS garante isolamento mesmo se código falhar
CREATE POLICY tenant_isolation ON users
  USING (infoapp_id = current_setting('app.current_tenant')::uuid);
```

### 3.4. PROS

✅ **Simplicidade**: Um único banco de dados, fácil de configurar
✅ **Custo**: Menos recursos de infraestrutura
✅ **Backup**: Backup unificado de todos os InfoApps
✅ **Analytics cross-app**: Fácil fazer queries agregadas
✅ **Shared resources**: Cache, conexões, pools compartilhados
✅ **Migrações**: Uma migração aplica para todos os tenants
✅ **Desenvolvimento**: Ambiente local simples (um DB)

### 3.5. CONS

❌ **Risco de query sem tenant_id**: Bug pode expor dados de outro InfoApp
❌ **Noisy neighbor**: InfoApp com muito tráfego afeta outros
❌ **Escala vertical**: Difícil escalar horizontalmente
❌ **Compliance**: Mais difícil isolar dados para LGPD (ex: deletar todos os dados de um InfoApp)
❌ **Restore**: Não pode restaurar backup de apenas 1 InfoApp
❌ **Database size**: Um DB gigante pode ter performance degradada

### 3.6. Mitigações

- **RLS (Row-Level Security)**: PostgreSQL força isolamento a nível de DB
- **ORM com tenant-scoping automático**: Django-tenants, Rails Acts-as-Tenant
- **Testes rigorosos**: Toda query deve ser testada com múltiplos tenants
- **Alertas**: Monitoring para queries sem tenant_id
- **Índices**: `(infoapp_id, ...)` em todas as tabelas

### 3.7. Quando usar

- Produto MVP/v1
- Poucos InfoApps (< 1.000)
- InfoApps de tamanho similar (sem "mega InfoApps")
- Time pequeno (simplicidade > robustez)

---

## 4. OPÇÃO B: DATABASE ISOLADO POR INFOAPP

### 4.1. Estrutura

```
┌────────────────────────────────────────────┐
│   PostgreSQL Cluster (múltiplos schemas)   │
│                                            │
│  ┌──────────────────────────────────────┐ │
│  │  Schema: infoapp_abc123              │ │
│  │    users                             │ │
│  │    lessons                           │ │
│  │    progress                          │ │
│  │    ...                               │ │
│  └──────────────────────────────────────┘ │
│                                            │
│  ┌──────────────────────────────────────┐ │
│  │  Schema: infoapp_def456              │ │
│  │    users                             │ │
│  │    lessons                           │ │
│  │    ...                               │ │
│  └──────────────────────────────────────┘ │
│                                            │
│  ┌──────────────────────────────────────┐ │
│  │  Schema: public (global)             │ │
│  │    infoapps                          │ │
│  │    creators                          │ │
│  │    billing                           │ │
│  └──────────────────────────────────────┘ │
└────────────────────────────────────────────┘
```

**Alternativa: Banco de dados separado por InfoApp** (mais robusto, mais caro)

### 4.2. Funcionamento

**Cada InfoApp tem schema próprio** (ou DB próprio)

**Conexão dinâmica**:
```python
# Request entra em "aprenda-python.plataforma.com"
# Middleware identifica: infoapp_id = "abc123"
# Conecta em schema "infoapp_abc123"
# Queries não precisam de WHERE infoapp_id (isolamento automático)
```

### 4.3. Implementação técnica

**PostgreSQL Schemas**:
```sql
-- Criar schema para novo InfoApp
CREATE SCHEMA infoapp_abc123;

-- Criar tabelas no schema
SET search_path TO infoapp_abc123;
CREATE TABLE users (...);
CREATE TABLE lessons (...);
```

**Django/Rails**:
```python
# Django com django-db-multitenant
DATABASES = {
    'default': {...},  # Schema público
    # Schemas de tenants criados dinamicamente
}

# Middleware roteia para schema correto
```

### 4.4. PROS

✅ **Isolamento total**: Impossível query cruzada entre InfoApps
✅ **Compliance**: Fácil deletar/exportar dados de um InfoApp
✅ **Performance**: InfoApp não afeta outros (noisy neighbor isolado)
✅ **Backup/Restore**: Backup e restore por InfoApp
✅ **Escala horizontal**: Pode mover InfoApps para DBs separados
✅ **Segurança**: Breach em um InfoApp não afeta outros
✅ **Migrações seletivas**: Pode migrar InfoApps em ondas

### 4.5. CONS

❌ **Complexidade**: Mais difícil de implementar e debugar
❌ **Migrações**: Precisa rodar migração em cada schema (lento)
❌ **Analytics cross-app**: Difícil fazer queries agregadas
❌ **Custo**: Mais conexões, mais recursos
❌ **Backup**: Backups múltiplos (mais storage)
❌ **Desenvolvimento**: Ambiente local mais complexo

### 4.6. Mitigações

- **Automação**: Scripts para criar schemas, rodar migrações
- **Analytics DB**: Replica dados agregados para warehouse separado
- **Pooling**: Connection pooling inteligente (PgBouncer)
- **Monitoring**: Tracking de schemas criados, migrações pendentes

### 4.7. Quando usar

- Produto maduro (v2+)
- Muitos InfoApps (1.000+)
- InfoApps de tamanhos variados (alguns gigantes)
- Requisitos fortes de compliance/isolamento
- Time grande (pode lidar com complexidade)

---

## 5. OPÇÃO C: ARQUITETURA HÍBRIDA

### 5.1. Estrutura

```
┌────────────────────────────────────────────┐
│      PostgreSQL - Platform DB (shared)     │
│                                            │
│  infoapps                                  │
│  creators                                  │
│  billing                                   │
│  analytics_aggregated                      │
└────────────────────────────────────────────┘

┌────────────────────────────────────────────┐
│   PostgreSQL Cluster - InfoApps (isolated) │
│                                            │
│  Schema: infoapp_abc123                    │
│  Schema: infoapp_def456                    │
│  Schema: infoapp_ghi789                    │
│  ...                                       │
└────────────────────────────────────────────┘
```

### 5.2. Funcionamento

**Dois bancos de dados**:
1. **Platform DB (shared)**: Dados globais (creators, billing, infoapps registry)
2. **InfoApps DB (isolated)**: Dados de cada InfoApp (schemas isolados)

**Fluxo**:
```
1. Request entra em "aprenda-python.plataforma.com"
2. Middleware consulta Platform DB: qual infoapp_id?
3. Conecta em schema do InfoApp no InfoApps DB
4. Queries de learner/lessons são isoladas
```

### 5.3. PROS

✅ **Melhor dos dois mundos**: Dados globais simples + InfoApps isolados
✅ **Flexibilidade**: Pode mover InfoApps grandes para DBs dedicados
✅ **Analytics**: Platform DB tem dados agregados
✅ **Compliance**: InfoApps isolados facilitam LGPD
✅ **Escala**: InfoApps DB pode escalar horizontalmente

### 5.4. CONS

❌ **Complexidade**: Gerenciar 2 bancos de dados
❌ **Joins cross-DB**: Não pode JOIN entre Platform DB e InfoApp DB
❌ **Transações**: Transações cross-DB são complexas
❌ **Migrações**: Precisa migrar 2 bancos separadamente

### 5.5. Quando usar

- Produto escalável de longo prazo
- Quer simplicidade para dados globais + isolamento para InfoApps
- Prevê crescimento grande (10.000+ InfoApps)

---

## 6. COMPARAÇÃO LADO A LADO

| Critério | Opção A (Shared DB) | Opção B (Isolated DB) | Opção C (Hybrid) |
|----------|---------------------|----------------------|------------------|
| **Simplicidade** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Isolamento** | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Performance** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Escalabilidade** | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Compliance** | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Custo** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Analytics cross-app** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ |
| **Desenvolvimento** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |

---

## 7. RECOMENDAÇÃO DO TECH LEAD

### 7.1. Para v1: **OPÇÃO C (HÍBRIDA)**

**Justificativa**:
1. **Conceito do produto**: Gerador de EdTech SaaS → muitos InfoApps criados → precisa escalar
2. **Isolamento**: Cliente enfatizou que InfoApp é "estrutura isolada" → Opção A é arriscada
3. **Compliance**: LGPD exige facilidade de deletar/exportar dados por InfoApp
4. **Escalabilidade**: Produto pode crescer muito → Opção B/C são melhores
5. **Complexidade gerenciável**: Opção C não é muito mais complexa que A

**Estrutura recomendada**:
```
Platform DB (PostgreSQL shared):
  - creators (usuários do Modo Studio)
  - infoapps (registry de InfoApps criados)
  - billing (planos, pagamentos)
  - analytics_aggregated (métricas cross-app)

InfoApps DB (PostgreSQL com schemas):
  - Schema por InfoApp: infoapp_<uuid>
  - Tabelas: users, lessons, beats, checkpoints, progress, badges, coins, etc.
```

### 7.2. Stack técnico sugerido

**Backend**: Django (Python) ou Rails (Ruby)
- Django: `django-tenants` ou `django-db-multitenant`
- Rails: `apartment` gem

**Database**: PostgreSQL
- Platform DB: Heroku Postgres Standard
- InfoApps DB: Heroku Postgres Premium (ou AWS RDS Multi-AZ)

**ORM**: Django ORM ou ActiveRecord
- Automação de schema switching
- Row-Level Security (RLS) como fallback

**Middleware**:
```python
# Pseudocódigo
class TenantMiddleware:
    def process_request(request):
        subdomain = request.get_host().split('.')[0]  # "aprenda-python"
        infoapp = InfoApp.objects.get(subdomain=subdomain)  # Platform DB
        set_current_schema(f"infoapp_{infoapp.id}")  # InfoApps DB
```

### 7.3. Migração futura (v2+)

Se produto escalar muito (10.000+ InfoApps):
- Migrar InfoApps grandes para DBs dedicados (sharding)
- Manter InfoApps pequenos em schemas compartilhados
- Routing layer decide qual DB usar por InfoApp

---

## 8. IMPLEMENTAÇÃO EM FASES

### Fase 1: v1 (MVP) - 3 meses
- Platform DB: creators, infoapps, billing
- InfoApps DB: schemas compartilhados (max 100 InfoApps)
- Middleware básico de tenant switching
- Sem otimizações avançadas

### Fase 2: v1.1 (Escala) - 6 meses
- InfoApps DB: suporta 1.000 schemas
- Connection pooling (PgBouncer)
- Analytics DB (replica read-only)
- Backup automático por schema

### Fase 3: v2 (Enterprise) - 12 meses
- Sharding: InfoApps grandes → DBs dedicados
- Multi-region: InfoApps por geolocalização
- Disaster recovery
- Compliance avançado (HIPAA, SOC2)

---

## 9. RISCOS E MITIGAÇÕES

### Risco 1: Muitos schemas degradam performance PostgreSQL
**Mitigação**:
- PostgreSQL suporta 10.000+ schemas sem problemas
- Benchmark com 1.000 schemas antes do launch
- Plano de sharding se atingir limite

### Risco 2: Migrações lentas (precisa rodar em cada schema)
**Mitigação**:
- Scripts automatizados de migração
- Migrar schemas em paralelo (background workers)
- Blue-green deployment por schema

### Risco 3: Analytics cross-app ficam lentos
**Mitigação**:
- Analytics DB separado (replica agregada)
- ETL noturno: dados de InfoApps → Analytics DB
- Use BigQuery/Snowflake para analytics pesados

### Risco 4: Complexidade para desenvolvedores
**Mitigação**:
- Documentação clara de como criar schemas
- Scripts de setup: `make create_tenant`
- Ambiente local com 3-5 schemas de teste

---

## 10. ALTERNATIVAS CONSIDERADAS E DESCARTADAS

### Alternativa 1: MongoDB com collections por tenant
**Descartado**: Produto precisa de transações ACID, relações complexas (lessons → beats → checkpoints). PostgreSQL é melhor.

### Alternativa 2: Microservices por InfoApp
**Descartado**: Over-engineering para v1. Muito complexo, caro, lento de desenvolver.

### Alternativa 3: Serverless (Lambda + DynamoDB)
**Descartado**: Lock-in AWS, difícil de debugar, cold starts afetam UX.

---

## 11. DECISÃO FINAL

### ✅ ARQUITETURA ESCOLHIDA: OPÇÃO C (HÍBRIDA)

**Platform DB** (shared):
- creators
- infoapps
- billing
- analytics_aggregated

**InfoApps DB** (isolated schemas):
- Schema por InfoApp: `infoapp_<uuid>`
- Tabelas: users, lessons, beats, checkpoints, progress, badges, coins, store_products, purchases, etc.

**Tech stack**:
- Django + django-tenants (ou Rails + apartment)
- PostgreSQL 15+
- PgBouncer (connection pooling)
- Redis (cache)
- Celery (background jobs para migrações)

**Timeline**: Implementar em v1 (primeiros 3 meses)

---

## 12. PERGUNTAS PARA O CLIENTE

1. **Previsão de escala**: Quantos InfoApps espera criar no primeiro ano? (10? 100? 1.000?)
2. **InfoApps gigantes**: Algum InfoApp pode ter 100.000+ alunos? Ou maioria será pequeno (< 1.000 alunos)?
3. **Compliance**: Precisa de certificações específicas (HIPAA, SOC2)? Ou LGPD básica é suficiente?
4. **Budget**: Qual orçamento mensal para infraestrutura? (Opção C é ~2x mais cara que A)

---

**Decisão tomada por**: Tech Lead
**Revisado por**: [Aguardando]
**Aprovado por**: [Aguardando validação do cliente]
