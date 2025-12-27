# Perguntas Adicionais (Arquitetura Open Source)

**Versão**: 1.0
**Data**: 2025-12-27
**Status**: AGUARDANDO RESPOSTA DO CLIENTE
**Contexto**: Clarificações críticas após respostas sobre open source + GCP

---

## COMO USAR ESTE DOCUMENTO

Este documento contém dúvidas que surgiram após suas respostas sobre open source, hospedagem e gamificação.

Para cada pergunta:
- ✅ = Confirmado/Correto
- ❌ = Incorreto/Precisa ajustar
- 🟡 = Parcialmente correto (explicar)
- ❓ = Não sei/Preciso pensar

**Instruções**: Cliente, por favor marque cada item e adicione observações onde necessário.

---

## 1. "RODAR NO GITHUB" - O QUE SIGNIFICA?

**Você disse**: "poderia rodar no github tmb"

**Nossa dúvida**: GitHub tem várias funcionalidades. Qual você quer dizer?

**Opções possíveis**:

### Opção A: GitHub Pages (deploy frontend estático)
- GitHub Pages hospeda sites estáticos gratuitamente
- **Limitação**: Apenas frontend (HTML/CSS/JS), SEM backend
- **Funciona para**: Landing page, documentação
- **NÃO funciona para**: InfoApp completo (precisa backend + DB)

[ ] ✅ Sim, quero GitHub Pages para documentação/landing page
[ ] ❌ Não era isso que eu quis dizer

---

### Opção B: GitHub Actions (CI/CD automático)
- GitHub Actions roda testes e deploy automaticamente quando você faz commit
- **Exemplo**: Commit no código → GitHub Actions testa → Deploy automático no GCP
- **Benefício**: Automação completa (push → deploy)

[ ] ✅ Sim, quero CI/CD com GitHub Actions (deploy automático)
[ ] ❌ Não era isso que eu quis dizer

---

### Opção C: GitHub Codespaces (ambiente de dev cloud)
- GitHub Codespaces é VS Code na nuvem
- **Benefício**: Você abre o projeto no navegador e desenvolve sem instalar nada
- **Custo**: 60h grátis/mês, depois paga

[ ] ✅ Sim, quero Codespaces para desenvolver sem instalar nada localmente
[ ] ❌ Não era isso que eu quis dizer

---

### Opção D: GitHub como repositório do código (open source)
- GitHub apenas armazena o código-fonte (público ou privado)
- Qualquer pessoa pode clonar e rodar localmente
- **Benefício**: Open source, transparência, comunidade pode contribuir

[ ] ✅ Sim, quero GitHub apenas para versionar o código (isso é obrigatório)
[ ] ❌ Não era isso que eu quis dizer

---

### Opção E: Outra coisa

**Explique o que você quer dizer com "rodar no GitHub"**:
_______________________________________________________________
_______________________________________________________________

---

## 2. OPEN SOURCE SEM BILLING - COMO CRIADOR GANHA DINHEIRO?

**Você disse**: "Grátis para todos (open source, sem billing)"

**Nossa dúvida**: Se a plataforma é grátis e open source, como o criador monetiza?

### Cenário A: InfoApp é 100% grátis para alunos (sem monetização)
- InfoApp é gratuito para todos os alunos
- Criador NÃO ganha dinheiro via plataforma
- Criador monetiza FORA da plataforma (curso presencial, consultoria, mentorias, etc.)
- Loja de recompensas é apenas gamificação (produtos virtuais)

[ ] ✅ Sim, InfoApp é totalmente grátis (criador não monetiza via plataforma)
[ ] ❌ Não, criador precisa poder cobrar alunos

---

### Cenário B: Criador vende produtos FORA da plataforma
- InfoApp é grátis, mas criador usa plataforma como **funil de aquisição**
- Loja de recompensas oferece **cupons de desconto** para produtos externos (curso avançado, ebook, consultoria)
- Aluno gasta Coins para ganhar cupom → compra produto FORA da plataforma (Hotmart, Eduzz, etc.)

[ ] ✅ Sim, criador usa InfoApp como funil para vender produtos externos
[ ] ❌ Não era isso que eu quis dizer

---

### Cenário C: Loja de recompensas aceita pagamento real (Coins + Reais)
- Aluno pode comprar produtos na loja com **Coins (gamificação)** OU **Reais (pagamento real)**
- **Exemplo**: Ebook custa 100 Coins OU R$ 10 reais
- Criador recebe pagamento pelos produtos vendidos com dinheiro real
- Plataforma cobra comissão (ex: 10%)

[ ] ✅ Sim, loja aceita Coins OU pagamento real
[ ] ❌ Não, loja é apenas Coins (sem dinheiro real)

---

### Cenário D: Open source MAS criador pode cobrar acesso ao InfoApp
- Código é open source (qualquer um pode ver/clonar)
- MAS criador pode cobrar acesso ao InfoApp hospedado (tipo WordPress: código aberto, mas você paga por hospedagem gerenciada)
- **Exemplo**: Aluno paga R$ 50/mês para acessar InfoApp do criador

[ ] ✅ Sim, código open source mas criador pode cobrar acesso
[ ] ❌ Não, acesso é sempre grátis

---

### Cenário E: Outro modelo

**Explique como criador ganha dinheiro**:
_______________________________________________________________
_______________________________________________________________

---

## 3. GOOGLE CLOUD NÃO É GRÁTIS - QUEM PAGA?

**Você disse**: "Roda em servidor cloud (GCP)" + "Grátis para todos"

**Nossa dúvida**: Google Cloud tem free tier (300 USD crédito inicial + Always Free tier), mas depois do free tier **cobra por uso**.

**Exemplo de custos mensais** (após free tier):
- Cloud Run (backend): ~$10-20/mês
- Cloud SQL (banco de dados): ~$10-30/mês
- Cloud Storage (arquivos): ~$5-10/mês
- **Total estimado**: $25-60/mês (~R$ 125-300/mês)

**Pergunta**: Quem paga essa conta?

---

### Opção A: VOCÊ (cliente) paga GCP para hospedar plataforma central
- Você mantém um servidor GCP rodando
- Todos os InfoApps rodam nesse servidor
- Você paga a conta do GCP (após free tier)

[ ] ✅ Sim, eu pago GCP para hospedar plataforma
[ ] ❌ Não, não quero pagar conta recorrente

---

### Opção B: Cada CRIADOR paga seu próprio GCP (self-hosted)
- Código é open source
- Cada criador clona o código e hospeda no próprio GCP
- Cada criador paga sua própria conta GCP
- Você (cliente) NÃO paga nada (apenas mantém código no GitHub)

[ ] ✅ Sim, cada criador hospeda e paga próprio GCP
[ ] ❌ Não, quero centralizar hospedagem

---

### Opção C: Apenas free tier (depois cada um se vira)
- Você fornece instruções para hospedar no GCP free tier
- Após free tier acabar (12 meses ou 300 USD), cada criador decide:
  - Pagar GCP
  - Migrar para outro servidor (AWS, Heroku, DigitalOcean)
  - Hospedar localmente

[ ] ✅ Sim, apenas documentar free tier (sem compromisso de hospedagem)
[ ] ❌ Não, precisa ter solução de hospedagem clara

---

### Opção D: Hospedar localmente (localhost) é suficiente
- Criador roda InfoApp no próprio computador (localhost)
- Alunos acessam via rede local OU você configura túnel (ngrok)
- **Limitação**: Não é profissional, não escala, depende do computador estar ligado

[ ] ✅ Sim, localhost é suficiente (não precisa cloud)
[ ] ❌ Não, precisa rodar em cloud (24/7)

---

### Opção E: Outra solução

**Explique quem paga hospedagem**:
_______________________________________________________________
_______________________________________________________________

---

## 4. NÍVEIS SEM XP - COMO FUNCIONAM?

**Você disse**: "Apenas Coins (não existe XP)"

**Nossa dúvida**: Se não tem XP, como funcionam **níveis** (Bronze, Prata, Ouro)?

---

### Opção A: Níveis baseados em Coins ACUMULADOS (lifetime)
- Aluno acumula Coins ao longo do tempo
- **Coins acumulados** nunca diminui (mesmo gastando)
- **Coins disponíveis** (saldo) diminui ao comprar na loja
- Níveis baseados em Coins acumulados:
  - Bronze: 0-100 coins acumulados
  - Prata: 101-500 coins
  - Ouro: 501-2.000 coins
  - Diamante: 2.001-10.000 coins
  - Lenda: 10.001+ coins

**Exemplo**:
- Aluno ganhou 600 coins (lifetime) → Nível: Ouro
- Aluno gastou 400 coins na loja → Saldo: 200 coins
- **Nível continua Ouro** (baseado em 600 lifetime, não saldo)

[ ] ✅ Sim, níveis baseados em Coins acumulados (lifetime)
[ ] ❌ Não, isso fica confuso

---

### Opção B: NÃO tem níveis (apenas Coins e Badges)
- Sistema simplificado: apenas Coins + Badges
- **Coins**: Moeda para gastar na loja
- **Badges**: Conquistas (completar módulo, streak 7 dias, etc.)
- **SEM níveis** (Bronze/Prata/Ouro removidos)

[ ] ✅ Sim, remover níveis (apenas Coins + Badges)
[ ] ❌ Não, quero ter níveis

---

### Opção C: Níveis baseados em PROGRESSO (módulos completados)
- Níveis baseados em quantas lições/módulos completou
- **Exemplo**:
  - Bronze: 0-10 lições completadas
  - Prata: 11-30 lições
  - Ouro: 31-100 lições
  - Diamante: 101-300 lições
  - Lenda: 300+ lições
- **Independente de Coins** (níveis = progresso, Coins = moeda)

[ ] ✅ Sim, níveis baseados em lições completadas
[ ] ❌ Não, prefiro outra opção

---

### Opção D: Outra ideia

**Explique como níveis funcionam**:
_______________________________________________________________
_______________________________________________________________

---

## 5. LOJA DE RECOMPENSAS - PRODUTOS SÃO PAGOS COM O QUÊ?

**Pergunta**: Produtos da loja são comprados com Coins (gamificação) OU Reais (dinheiro real)?

---

### Opção A: Apenas Coins (sem dinheiro real)
- Todos os produtos custam Coins
- Aluno ganha Coins estudando
- Aluno gasta Coins na loja
- **SEM pagamento real** (100% gamificação)

[ ] ✅ Sim, apenas Coins (sem dinheiro real)
[ ] ❌ Não, quero permitir pagamento real

---

### Opção B: Coins OU Reais (aluno escolhe)
- Produtos podem custar Coins OU Reais
- **Exemplo**: Ebook custa **100 Coins** OU **R$ 10**
- Aluno escolhe como pagar
- Criador recebe dinheiro real (se pagar em Reais)

[ ] ✅ Sim, Coins OU Reais (duplo preço)
[ ] ❌ Não, apenas Coins

---

### Opção C: Apenas Reais (Coins são apenas progresso)
- Loja aceita apenas pagamento real (R$, USD, etc.)
- Coins servem apenas para desbloquear acesso à loja (ex: precisa 100 Coins para poder comprar)

[ ] ✅ Sim, apenas Reais (Coins desbloqueiam produtos)
[ ] ❌ Não, prefiro outra opção

---

### Opção D: Outra ideia

**Explique como funciona**:
_______________________________________________________________
_______________________________________________________________

---

## RESUMO DAS PERGUNTAS

Para facilitar, resuma suas escolhas aqui:

**1. "Rodar no GitHub" significa**:
- [ ] GitHub Pages (landing page)
- [ ] GitHub Actions (CI/CD)
- [ ] GitHub Codespaces (dev cloud)
- [ ] GitHub como repositório (open source)
- [ ] Outro: _______________

**2. Como criador ganha dinheiro**:
- [ ] InfoApp é 100% grátis (sem monetização)
- [ ] Criador vende produtos externos (funil)
- [ ] Loja aceita Coins OU Reais
- [ ] Código open source mas cobra acesso
- [ ] Outro: _______________

**3. Quem paga GCP**:
- [ ] Você (cliente) paga GCP central
- [ ] Cada criador paga próprio GCP (self-hosted)
- [ ] Apenas free tier (sem compromisso)
- [ ] Localhost é suficiente
- [ ] Outro: _______________

**4. Níveis funcionam assim**:
- [ ] Baseados em Coins acumulados (lifetime)
- [ ] Sem níveis (apenas Coins + Badges)
- [ ] Baseados em lições completadas
- [ ] Outro: _______________

**5. Produtos da loja são pagos com**:
- [ ] Apenas Coins (gamificação)
- [ ] Coins OU Reais (duplo preço)
- [ ] Apenas Reais
- [ ] Outro: _______________

---

## PRÓXIMOS PASSOS (APÓS RESPOSTA)

1. ✅ Cliente responde perguntas
2. Time ajusta arquitetura (GCP, gamificação, monetização)
3. Criar guias de setup (localhost, GCP, GitHub)
4. Atualizar roadmap open source
5. Validar com cliente antes de implementar

---

**Criado por**: Product Architect + Tech Lead + DevOps
**Aguardando validação de**: CLIENTE
**Prazo de resposta sugerido**: 2-3 dias úteis

---

## OBSERVAÇÕES FINAIS DO CLIENTE

Use este espaço para qualquer observação adicional ou contexto que ajude a esclarecer suas respostas:

_______________________________________________________________
_______________________________________________________________
_______________________________________________________________
_______________________________________________________________
