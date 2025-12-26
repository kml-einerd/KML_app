# DÚVIDAS PARA VALIDAÇÃO COM CLIENTE

**Novas questões técnicas e de produto que surgiram das respostas anteriores**

Este documento consolida NOVAS dúvidas que o time de especialistas identificou ao implementar as decisões do cliente. São questões críticas que afetam arquitetura, experiência do usuário e modelo de negócio.

[fonte: Respostas Cliente #1-15 → Novas dúvidas surgidas da implementação]

---

## 1. LOJA DE RECOMPENSAS (MARKETPLACE)

### 1.1 Configuração de Produtos

**Contexto**: Loja de recompensas permite alunos trocarem XP/moedas por produtos/benefícios. Mas quem configura esses produtos?

**Dúvida**:
- **Opção A**: Platform Admin configura catálogo GLOBAL (todos os apps compartilham mesmos produtos)
- **Opção B**: Cada Creator configura produtos para SEU app (personalização por marca)
- **Opção C**: Híbrido (Platform Admin oferece produtos padrão + Creator pode adicionar produtos custom)

**Implicações**:
- Opção A: Simples, mas menos flexível (criador não controla o que oferecer)
- Opção B: Flexível, mas complexo (cada criador precisa configurar produtos, integrar fulfillment)
- Opção C: Melhor de ambos, mas mais complexo tecnicamente

**Recomendação do Time**: Opção C (híbrido) para v1, permitindo escalabilidade.

**Pergunta para Cliente**: Quem configura os produtos da loja? Platform Admin, Creator, ou ambos?

---

### 1.2 Tipos de Produtos/Benefícios

**Contexto**: Loja pode ter diversos tipos de produtos. Precisamos definir quais tipos suportar em v1.

**Dúvida**: Quais tipos de produtos/benefícios devem estar disponíveis?

**Sugestões**:
- ✅ Descontos em cursos (cupom)
- ✅ Acesso premium temporário (7/30/90 dias)
- ✅ Gift cards (R$10, R$50, R$100)
- ❓ Merchandise físico (camiseta, caneca, etc) → Requer integração fulfillment
- ❓ Sessões 1:1 com criador → Requer agendamento
- ❓ Conteúdo exclusivo (ebook, template) → Requer download/entrega digital
- ❓ Early access a novos cursos → Requer controle de acesso

**Pergunta para Cliente**: Quais tipos de produtos devem estar na loja em v1? Apenas digitais ou também físicos?

---

### 1.3 Conversão XP → Moedas

**Contexto**: Aluno ganha XP ao completar atividades. XP pode ser convertido em moedas para comprar produtos.

**Dúvida**: Qual a fórmula de conversão?
- **Opção A**: 1 XP = 1 Moeda (1:1, simples)
- **Opção B**: 10 XP = 1 Moeda (escala maior, previne inflação)
- **Opção C**: Fórmula custom por app (criador define)

**Implicações**:
- 1:1: Simples, mas pode gerar inflação de moedas (aluno acumula muito rápido)
- 10:1: Escala maior, controla inflação, mas pode parecer "difícil" para aluno
- Custom: Flexível, mas complexo (criador pode quebrar economia)

**Recomendação do Time**: Iniciar com 10:1 fixo (v1), permitir custom em v1.1+.

**Pergunta para Cliente**: Conversão XP → Moedas é 1:1, 10:1, ou outro valor?

---

### 1.4 Integração Fulfillment (Produtos Físicos)

**Contexto**: Se loja oferece merchandise físico (camiseta, caneca), precisa integrar com serviço de fulfillment.

**Dúvida**: Integração com fulfillment está em v1 ou v1.5?

**Opções de Fulfillment**:
- Printful (print-on-demand)
- Shopify (via API)
- Manual (criador envia por conta própria)

**Recomendação do Time**:
- v1: Apenas produtos digitais (sem fulfillment)
- v1.5: Integração com Printful ou Shopify

**Pergunta para Cliente**: Produtos físicos devem estar em v1 ou podem esperar v1.5?

---

## 2. APLICAÇÃO SEM UPLOAD (VALIDAÇÃO)

### 2.1 Como Validar Honestidade?

**Contexto**: Aplicação agora é auto-declarativa (checklist + texto/link). Sem upload, como garantir que aluno realmente fez a tarefa?

**Dúvida**: Qual mecanismo de validação usar?

**Opções**:
- **Opção A**: Honestidade pura (aluno marca como feito, sem validação)
- **Opção B**: Peer review (outro aluno revisa e aprova)
- **Opção C**: Criador aprova manualmente (amostra ou 100%)
- **Opção D**: Análise de padrões suspeitos (texto copiado, links spam) → automático

**Implicações**:
- Opção A: Simples, mas alta chance de fraude
- Opção B: Engajamento social, mas complexo (quem revisa? critérios?)
- Opção C: Qualidade alta, mas não escala (criador sobrecarregado)
- Opção D: Escalável, mas requer ML/heurísticas

**Recomendação do Time**: Opção A (honestidade) + D (padrões suspeitos) em v1. Opção B (peer review) em v1.5.

**Pergunta para Cliente**: Como validar que aluno fez a aplicação sem upload? Honestidade, peer review, ou aprovação manual do criador?

---

### 2.2 Rubrica de Avaliação Ainda Existe?

**Contexto**: Versão anterior tinha "rubrica" (critérios objetivos de avaliação). Com auto-declaração, rubrica ainda faz sentido?

**Dúvida**: Aplicação ainda tem rubrica ou apenas "marcar como feito"?

**Opções**:
- **Opção A**: Rubrica automática (baseada em checklist completo)
- **Opção B**: Sem rubrica (apenas "feito" ou "não feito")
- **Opção C**: Feedback coaching genérico (sempre positivo)

**Recomendação do Time**: Opção C (feedback coaching) para manter diferenciação científica.

**Pergunta para Cliente**: Aplicação ainda tem rubrica de avaliação ou apenas feedback encorajador?

---

### 2.3 Aplicação Bloqueia Progresso?

**Contexto**: Cientista de Aprendizagem exige "Aplicação por módulo obrigatória". Mas se é auto-declarativa, bloqueia progresso ou é opcional?

**Dúvida**: Aluno PRECISA completar aplicação para avançar ou pode pular?

**Opções**:
- **Opção A**: Obrigatório (bloqueia próximo módulo até completar)
- **Opção B**: Opcional (aluno pode pular, mas perde XP)
- **Opção C**: Soft-lock (pode avançar, mas vê aviso "você pulou aplicação")

**Recomendação do Time**: Opção A (obrigatório) para manter rigor científico.

**Pergunta para Cliente**: Aplicação bloqueia progresso (obrigatório) ou aluno pode pular?

---

## 3. MULTI-IDIOMA (i18n)

### 3.1 Conteúdo do Creator é Multi-Idioma?

**Contexto**: Interface do app terá i18n (PT-BR/EN/ES). Mas e o conteúdo criado pelo Creator (aulas, missões)?

**Dúvida**: Conteúdo criado é em 1 idioma ou multi-idioma?

**Opções**:
- **Opção A**: 1 idioma apenas (criador escolhe idioma do app, não traduz conteúdo)
- **Opção B**: Multi-idioma manual (criador traduz conteúdo para múltiplos idiomas)
- **Opção C**: Multi-idioma automático (IA traduz conteúdo, criador revisa)

**Implicações**:
- Opção A: Simples, mas limita alcance (app em PT-BR não atinge EN)
- Opção B: Flexível, mas trabalhoso (criador precisa traduzir TUDO)
- Opção C: Escalável, mas requer IA de tradução (custo + qualidade variável)

**Recomendação do Time**: Opção A em v1 (1 idioma), Opção B em v1.5+ (manual).

**Pergunta para Cliente**: Conteúdo criado pelo Creator é em 1 idioma ou multi-idioma? Se multi, quem traduz?

---

### 3.2 Interface: Quais Idiomas em v1?

**Contexto**: i18n implementado em v1 (infraestrutura). Mas quantos idiomas traduzir na interface?

**Dúvida**: Quais idiomas traduzir em v1?

**Opções**:
- **Opção A**: PT-BR apenas (simplifica)
- **Opção B**: PT-BR + EN (mercado global)
- **Opção C**: PT-BR + EN + ES (América Latina)

**Recomendação do Time**: Opção A (PT-BR) em v1, Opção B (EN) em v1.1.

**Pergunta para Cliente**: Interface do app em v1 é PT-BR apenas ou também EN/ES?

---

## 4. ARQUITETURA MULTI-APP

### 4.1 Subdomínio Automático ou Manual?

**Contexto**: Cada app criado no Studio vira um app separado. Como funciona o domínio?

**Dúvida**: Subdomínio é automático ou criador escolhe?

**Opções**:
- **Opção A**: Automático (ex: app123.plataforma.com.br)
- **Opção B**: Criador escolhe slug (ex: vsl-mastery.plataforma.com.br)
- **Opção C**: Criador pode conectar domínio próprio (ex: curso.meusite.com)

**Recomendação do Time**:
- v1: Opção B (slug customizável)
- v1.5: Opção C (domínio próprio em plano Full)

**Pergunta para Cliente**: Apps criados têm subdomínio automático (app123) ou criador escolhe slug (meu-app)?

---

### 4.2 Apps Compartilham Banco de Dados ou São Isolados?

**Contexto**: Multi-tenancy pode ser "shared database" (todos os apps no mesmo banco, separados por tenant_id) ou "isolated database" (cada app tem banco próprio).

**Dúvida**: Qual arquitetura usar?

**Opções**:
- **Opção A**: Shared database (1 banco, tenant_id em todas as tabelas)
- **Opção B**: Isolated database (1 banco por app)
- **Opção C**: Shared com opção de isolamento (plano Full tem banco isolado)

**Implicações**:
- Opção A: Simples, mas risco de vazamento de dados entre apps
- Opção B: Seguro, mas complexo (gerenciar múltiplos bancos)
- Opção C: Flexível, mas complexo

**Recomendação do Time**: Opção A (shared) em v1, Opção C (híbrido) em v1.5+.

**Pergunta para Cliente**: Apps compartilham banco de dados ou são isolados? (Time recomenda compartilhado em v1)

---

### 4.3 Criador Pode Conectar Domínio Próprio em Qual Plano?

**Contexto**: Criador pode querer usar domínio próprio (ex: curso.meusite.com) em vez de subdomínio da plataforma.

**Dúvida**: Domínio próprio está disponível em qual plano?

**Opções**:
- **Opção A**: Apenas plano Full
- **Opção B**: Standard e Full
- **Opção C**: Todos os planos (inclusive MicroSaaS)

**Recomendação do Time**: Opção A (Full apenas) para diferenciar planos.

**Pergunta para Cliente**: Domínio personalizado está disponível apenas no plano Full ou também no Standard?

---

## 5. ANALYTICS E NOTIFICAÇÕES

### 5.1 Dashboard Analytics: Custom ou Link Externo?

**Contexto**: Creator Studio precisa mostrar analytics (funil, drop-off, cohorts). Dashboard é custom (dentro do app) ou link externo (Google Analytics)?

**Dúvida**: Dashboard é custom ou link externo?

**Opções**:
- **Opção A**: Custom (dashboard dentro do Creator Studio)
- **Opção B**: Link externo (criador acessa Google Analytics)
- **Opção C**: Híbrido (métricas básicas custom + link para detalhes)

**Recomendação do Time**:
- v1: Opção C (métricas básicas custom + link para GA)
- v1.1: Opção A (dashboard completo)

**Pergunta para Cliente**: Dashboard de analytics é custom (dentro do app) ou link externo para Google Analytics?

---

### 5.2 Notificações: Email Apenas ou Push Gratuito?

**Contexto**: v1 tem notificações email (streak em risco, novas missões). v1.1 adiciona push SE houver solução gratuita.

**Dúvida**: Há solução de push gratuita viável?

**Opções gratuitas**:
- **OneSignal**: Free tier 10k push/mês
- **Firebase Cloud Messaging**: Free unlimited
- **Email apenas**: SendGrid/Mailgun free tier

**Recomendação do Time**:
- v1: Email apenas (SendGrid)
- v1.1: Push via Firebase (free unlimited)

**Pergunta para Cliente**: Email notificações são suficientes em v1 ou queremos push gratuito (Firebase) também?

---

## 6. TTS CACHE (DETALHES TÉCNICOS)

### 6.1 Cache TTL (Time To Live)

**Contexto**: TTS cache gera áudio UMA vez. Mas quanto tempo manter no cache?

**Dúvida**: Cache é eterno ou tem TTL?

**Opções**:
- **Opção A**: Eterno (cache nunca expira, economiza custo máximo)
- **Opção B**: TTL 30 dias (limpa caches antigos, economiza storage)
- **Opção C**: TTL custom por app (criador decide)

**Recomendação do Time**: Opção A (eterno) com invalidação manual quando criador atualiza conteúdo.

**Pergunta para Cliente**: Cache de TTS é eterno ou expira após X dias?

---

### 6.2 Invalidação de Cache Quando Criador Atualiza

**Contexto**: Se criador edita conteúdo (muda texto de narração), cache deve ser invalidado.

**Dúvida**: Invalidação é automática ou manual?

**Opções**:
- **Opção A**: Automática (qualquer edição no texto invalida cache)
- **Opção B**: Manual (criador clica "Regenerar áudio")
- **Opção C**: Versionada (novo áudio só gera em nova versão publicada)

**Recomendação do Time**: Opção C (versionada) para evitar invalidações acidentais.

**Pergunta para Cliente**: Criador pode regenerar áudio manualmente ou é automático ao editar texto?

---

## RESUMO: DECISÕES PRIORITÁRIAS

**ALTA PRIORIDADE (afetam arquitetura v1)**:
1. Loja: Quem configura produtos? (Platform Admin ou Creator?)
2. Loja: Tipos de produtos em v1? (Apenas digitais ou também físicos?)
3. Aplicação: Como validar honestidade? (Honestidade pura ou peer review?)
4. Aplicação: Bloqueia progresso? (Obrigatório ou opcional?)
5. Multi-App: Apps compartilham banco ou isolados?
6. i18n: Conteúdo é 1 idioma ou multi? Quem traduz?

**MÉDIA PRIORIDADE (podem ser decididas durante desenvolvimento)**:
7. Conversão XP → Moedas (1:1 ou 10:1?)
8. Subdomínio: Automático ou criador escolhe slug?
9. Analytics: Dashboard custom ou link externo?
10. Notificações: Email apenas ou também push gratuito?

**BAIXA PRIORIDADE (podem ser v1.1+)**:
11. Fulfillment: v1 ou v1.5?
12. TTS Cache TTL: Eterno ou expira?
13. Domínio próprio: Qual plano?

---

**FORMATO DE RESPOSTA SUGERIDO PARA CLIENTE**:

```
1. Loja - Quem configura: [Platform Admin / Creator / Híbrido]
2. Loja - Tipos v1: [Apenas digitais / Também físicos]
3. Aplicação - Validação: [Honestidade / Peer review / Aprovação manual]
4. Aplicação - Obrigatório: [Sim, bloqueia / Não, opcional]
5. Multi-App - Banco: [Compartilhado / Isolado]
6. i18n - Conteúdo: [1 idioma / Multi-idioma manual / Multi-idioma IA]
7. Conversão Moedas: [1:1 / 10:1 / Outro: ___]
8. Subdomínio: [Automático / Criador escolhe]
9. Analytics Dashboard: [Custom / Link externo / Híbrido]
10. Notificações v1: [Email apenas / Email + Push Firebase]
```

---

**Última Atualização**: 2025-12-26
