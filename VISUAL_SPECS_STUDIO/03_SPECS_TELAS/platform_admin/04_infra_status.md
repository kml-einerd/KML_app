# INFRA / STATUS

## 1) Objetivo: Saúde do sistema, alertas de custo (ElevenLabs, storage), performance.
[fonte: 03 - todas as telas, flows e governança fechados.md → 2.3 Platform Admin → Infra/Status]

## 2) Usuário: Platform Admin, **Contexto**: Operação e monitoramento

## 3) Layout
```
**Saúde do Sistema**
✓ API: OK (200ms avg)
✓ Database: OK
⚠️ Storage: 85% (alerta)
❌ ElevenLabs: Quota 95% (crítico)

**Alertas de Custo**
🔴 ElevenLabs: R$ 1.200/mês (quota: R$ 1.500)
  Recomendação: Revisar cache de TTS

🟡 Storage: 425 GB (quota: 500 GB)
  Recomendação: Implementar cleanup de assets antigos
```

## 4) CTA Primária: "Ver Detalhes" (alertas)
## 5) Estados: Success (sempre visível)
## 6) Som: **SAFE**, alerta se crítico
## 7) Eventos: `infra_viewed`, `alert_acknowledged`
## 8) DoD: [ ] Saúde do sistema, [ ] Alertas de custo, [ ] Performance metrics
## 9) Edições: Admin apenas
## 10) Back: `GET /api/admin/infra/status`

[fonte: 01_CONSOLIDACAO_CONSELHO.md → FinOps → Alertas para estouro de custo]
---
