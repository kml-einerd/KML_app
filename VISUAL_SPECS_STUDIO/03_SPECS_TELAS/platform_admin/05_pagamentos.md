# PAGAMENTOS (PLATFORM ADMIN)

## 1) Objetivo: Gestão de planos, cobrança, chargebacks.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Pagamentos]

## 2) Usuário: Platform Admin, **Contexto**: Operação financeira

## 3) Layout
```
**Planos Ativos**
MicroSaaS: 45 workspaces - R$ 1.305/mês
Standard: 30 workspaces - R$ 2.970/mês
Full: 10 workspaces - R$ 2.990/mês

Total MRR: R$ 7.265

**Chargebacks (2)**
• VSL Academy - R$ 99 - 15/12/2025
  [Investigar] [Resolver]
```

## 4) CTA Primária: "Investigar" (chargebacks)
## 5) Estados: Success
## 6) Som: **SAFE**
## 7) Eventos: `payments_viewed`, `chargeback_investigated`
## 8) DoD: [ ] MRR exibido, [ ] Chargebacks listados, [ ] Ações funcionam
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/billing`

[fonte: 03 - todas as telas, flows e governança fechados.md → Platform Admin → Pagamentos]
---
