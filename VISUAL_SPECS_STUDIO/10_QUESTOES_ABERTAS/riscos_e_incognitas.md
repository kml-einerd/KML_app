# QUESTÕES ABERTAS E RISCOS

**Incógnitas inevitáveis que requerem decisões futuras**

[fonte: 00_README.md → Questões Abertas]

---

## 1. INTEGRAÇÕES EXTERNAS

### ElevenLabs TTS
**Questão**: Como gerenciar custo em escala?
**Risco**: Uso massivo pode explodir custo
**Mitigação proposta**:
- Cache agressivo de áudio gerado (não regerar)
- Quotas por plano (MicroSaaS: 1k chars, Standard: 5k, Full: 20k)
- Alertas quando quota > 80%

**Decisão pendente**: Fallback se quota estoura? (Bloquear ou usar TTS alternativo mais barato?)

---

### Assets/Storage
**Questão**: Como armazenar assets (imagens/SVG/vídeos) em escala?
**Risco**: Storage pode crescer rapidamente
**Mitigação proposta**:
- Limites por plano (MicroSaaS: 1GB, Standard: 5GB, Full: 50GB)
- Compressão automática de imagens
- Cleanup de assets não usados (após X dias)

**Decisão pendente**: CDN obrigatório desde v1? Ou apenas quando atingir escala?

---

## 2. PROVA DE APLICAÇÃO (Anti-Fraude)

**Questão**: Como validar que prova enviada (screenshot/upload) não é falsa?
**Risco**: Usuários podem fraudar (screenshots falsas, cópia de outros)
**Mitigação proposta**:
- Revisão manual de amostra (Platform Admin: Moderação/Trust)
- Machine learning para detectar screenshots obviamente falsas (futuro)
- Rubrica com critérios objetivos (reduz subjetividade)

**Decisão pendente**: Validação automática de prova é obrigatória em v1? Ou apenas revisão manual por amostra?

---

## 3. MARKETPLACE PÚBLICO (Curadoria)

**Questão**: Como escalar curadoria de marketplace sem equipe grande?
**Risco**: Apps ruins podem ser listados, prejudicando reputação
**Mitigação proposta**:
- QA Gates automáticos (bloqueiam apps ruins)
- Score de Qualidade (≥70 para marketplace)
- Review editorial por amostra (5% ou todos apps públicos)

**Decisão pendente**: Review editorial é 100% dos apps ou apenas amostra? Se amostra, qual critério de seleção?

---

## 4. NOTIFICAÇÕES PUSH (Retenção)

**Questão**: Notificações são obrigatórias em v1 ou podem ser adicionadas depois?
**Risco**: Sem notificações, retenção D1/D7 pode ser baixa
**Mitigação proposta**:
- Implementar notificações desde v1 (opt-in, não opt-out)
- Tipos: Streak em risco, Novas missões, Unlock de badge

**Decisão pendente**: Notificações são v1 ou v1.1? Se v1, qual serviço usar (Firebase, OneSignal, custom)?

---

## 5. DARK MODE

**Questão**: Dark mode é obrigatório em v1?
**Risco**: Se não implementar, usuários podem reclamar (acessibilidade)
**Mitigação proposta**:
- Tokens de design facilitam (swap de cores)
- Media query `prefers-color-scheme: dark`

**Decisão pendente**: Dark mode é v1 ou v1.1? Se v1, aumenta escopo significativamente.

---

## 6. MULTILINGUAGEM (i18n)

**Questão**: Produto será multi-idioma desde v1?
**Risco**: Se não implementar i18n desde início, adicionar depois é doloroso
**Mitigação proposta**:
- Usar biblioteca de i18n (react-i18next, next-intl)
- Começar com PT-BR, adicionar EN/ES depois

**Decisão pendente**: i18n é v1 (apenas infra, 1 idioma) ou v1.1 (múltiplos idiomas)?

---

## 7. MOBILE (Native vs Web)

**Questão**: App será Native (React Native/Flutter) ou Web (PWA/responsive)?
**Risco**: Native exige manutenção de 3 codebases (iOS/Android/Web)
**Mitigação proposta**:
- Web-first (PWA responsivo)
- Adicionar Native quando atingir tração

**Decisão pendente**: v1 é Web (PWA) ou Native? Se Web, PWA installable é obrigatório?

---

## 8. OFFLINE MODE

**Questão**: App precisa funcionar offline?
**Risco**: Sem offline, usuários em conexões ruins abandonam
**Mitigação proposta**:
- Cache de aulas/missões para offline (Service Worker)
- Sync quando voltar online

**Decisão pendente**: Offline mode é v1 ou nice-to-have? Se v1, escopo aumenta.

---

## 9. CHECKOUT/BILLING

**Questão**: Qual plataforma de pagamento usar?
**Risco**: Escolha errada pode gerar fricção ou altos custos
**Opções**:
- Stripe (global, fácil integração)
- PagSeguro/MercadoPago (Brasil)
- Custom (boleto/PIX)

**Decisão pendente**: Stripe é suficiente ou precisa múltiplas integrações? PIX é obrigatório?

---

## 10. SEO/DISCOVERY (Marketplace)

**Questão**: Como usuários descobrem InfoApps no marketplace?
**Risco**: Sem SEO/discovery, marketplace fica vazio
**Mitigação proposta**:
- SEO básico (meta tags, sitemap)
- Featured apps (curadoria)
- Search + filtros (nicho, rating)

**Decisão pendente**: SEO avançado (SSR/SSG) é v1 ou v1.1? Se SSR/SSG, framework muda (Next.js/Remix)?

---

## 11. COMPLIANCE (LGPD/GDPR)

**Questão**: Produto está 100% conforme LGPD/GDPR?
**Risco**: Multas pesadas se não conforme
**Mitigação proposta**:
- Consentimentos obrigatórios (Signup, Upload de prova)
- Política de privacidade clara
- Direito de exclusão de dados

**Decisão pendente**: Revisão legal é obrigatória antes de v1? Se sim, quando?

---

## 12. INFRA/DEVOPS

**Questão**: Onde hospedar? CI/CD desde v1?
**Opções**:
- Vercel/Netlify (Web, fácil)
- AWS/GCP (flexível, complexo)
- Render/Railway (meio-termo)

**Decisão pendente**: Infra é v1 concern ou pode ser simplificado inicialmente (ex: Vercel) e migrar depois?

---

## 13. ESCALABILIDADE (80+ APPS)

**Questão**: Produto foi projetado para escalar para 80+ apps?
**Risco**: Arquitetura ruim pode não aguentar escala
**Mitigação proposta**:
- Multi-tenancy desde v1 (workspaces isolados)
- Cache agressivo (Redis)
- CDN para assets

**Decisão pendente**: Load testing é v1 ou v1.1? Se v1, quando fazer?

---

## 14. ACESSIBILIDADE (WCAG AA)

**Questão**: Produto atinge WCAG AA 100%?
**Risco**: Se não conforme, exclui usuários com deficiência
**Mitigação proposta**:
- Contrast ratio mínimo (4.5:1)
- Focus visível sempre
- ARIA labels
- Reduce motion/sound

**Decisão pendente**: Auditoria de acessibilidade (ferramentas automatizadas + manual) é v1 ou v1.1?

---

## 15. ANALYTICS (Ferramental)

**Questão**: Qual ferramenta de analytics usar?
**Opções**:
- Amplitude (product analytics, caro)
- Mixpanel (similar, mais barato)
- PostHog (open-source, self-hosted)
- Custom (Clickhouse/BigQuery + Metabase)

**Decisão pendente**: Analytics tool é escolhido antes de v1 ou pode ser trocado depois (via abstração)?

---

## RESUMO: DECISÕES CRÍTICAS PENDENTES

**Alto Impacto (decidir antes de v1)**:
1. ElevenLabs: Fallback se quota estoura?
2. Marketplace: Review editorial 100% ou amostra?
3. Notificações Push: v1 ou v1.1?
4. Mobile: Native ou Web (PWA)?
5. Billing: Stripe suficiente ou múltiplas integrações?
6. Infra: Onde hospedar?

**Médio Impacto (pode decidir durante desenvolvimento)**:
7. Dark mode: v1 ou v1.1?
8. i18n: v1 (infra) ou v1.1 (múltiplos idiomas)?
9. Offline mode: v1 ou nice-to-have?
10. SEO avançado (SSR/SSG): v1 ou v1.1?

**Baixo Impacto (pode ser v1.1)**:
11. Compliance: Revisão legal timing
12. Load testing: v1 ou v1.1?
13. Acessibilidade auditoria: v1 ou v1.1?
14. Analytics tool: Escolha antecipada ou via abstração?

---

**Última Atualização**: 2025-12-26
