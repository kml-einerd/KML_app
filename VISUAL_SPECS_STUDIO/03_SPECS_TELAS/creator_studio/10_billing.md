# BILLING (⚠️ DEPRECATED - REMOVIDO EM v3.0)

**Status**: DEPRECATED
**Data de Deprecação**: 2025-12-27
**Razão**: Cliente decidiu por modelo open source sem billing

**Cliente disse**: "Grátis para todos (open source, sem billing)"

**Impacto**:
- Plataforma é open source (GitHub)
- Sem cobrança criador → plataforma
- Sem planos pagos (Free/Starter/Pro)
- Self-hosted (cada criador hospeda próprio servidor)

**Ver nova arquitetura**: `/13_ARQUITETURA_PRODUTO/arquitetura_gcp.md`

---

# BILLING (CONTEÚDO ANTIGO - APENAS REFERÊNCIA)

## 1) Objetivo da Tela
Gerenciar plano, quotas (TTS, storage, usuários), faturas.

[fonte: 03 - todas as telas, flows e governança fechados.md → Billing]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer ver plano/quota/faturas, **Frequência**: 1x por mês

## 3) Layout & Hierarquia
```
[Header: "Plano & Billing"]

Plano Atual: Standard
Próxima cobrança: 01/01/2026 • R$ 99/mês

[Quotas]
TTS (ElevenLabs): 2.500 / 5.000 caracteres (50%)
Storage: 1.2 GB / 5 GB (24%)
Usuários Ativos: 120 / 500 (24%)

[Alertas]
⚠️ Quota TTS em 80% - considere upgrade

[Planos Disponíveis]
MicroSaaS: R$ 29/mês
Standard: R$ 99/mês (atual)
Full: R$ 299/mês

[Botão: "Upgrade"]

[Faturas]
Dez/2025 - R$ 99 - Pago
Nov/2025 - R$ 99 - Pago
```

[fonte: 01_CONSOLIDACAO_CONSELHO.md → FinOps → Quotas por plano]

## 4) Elementos & Componentes
- **Quota Bars**: Progresso visual
- **Alertas**: Se quota > 80%
- **Planos**: Cards comparativos
- **Faturas**: Lista

## 5) Ação Primária
"Upgrade" (se quota alta ou quer mais features)

## 6) Estados
- **Success**: Plano/quotas exibidos

## 7) Conteúdo / Microcopy
- Quotas: "2.500 / 5.000 caracteres (50%)"
- Alertas: "Quota TTS em 80% - considere upgrade"

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`

## 9) Eventos
`billing_viewed`, `upgrade_clicked`

## 10) Definition of Done
- [ ] Quotas exibidas visualmente
- [ ] Alertas se quota > 80%
- [ ] Upgrade funcional
- [ ] Faturas listadas

## 11) Modo Studio / Edições
Todos mantêm (billing obrigatório para produto final)

## 12) Mapeamento Back
`GET /api/creator/workspace/:id/billing`
Integração: Stripe ou similar

## 13) Rastreabilidade
[fonte: 01_CONSOLIDACAO_CONSELHO.md → FinOps → Quotas e alertas]

---
