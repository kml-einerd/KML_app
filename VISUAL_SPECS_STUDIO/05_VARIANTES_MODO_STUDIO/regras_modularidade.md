# REGRAS DE MODULARIDADE

**Como modularizar features sem quebrar jornadas**

[fonte: 00_README.md → Modo Studio → Modularidade]

---

## PRINCÍPIO

Edições removem features, MAS:
- Jornadas principais continuam funcionando
- Nenhum "link quebrado" ou erro

---

## REGRAS

### 1. Feature Flags (Backend)
```json
{
  "edition": "MicroSaaS",
  "features": {
    "review_srs": false,
    "import_pack": false,
    "versioning": false
  }
}
```

### 2. Navegação (Frontend)
- Se feature desabilitada: Remover do menu/sidebar
- Não mostrar link para tela inexistente

### 3. Upsell (se usuário tenta acessar feature bloqueada)
```
┌─────────────────────────────┐
│ 🔒 Review SRS                │
│ Disponível no plano Standard│
│ [Upgrade]                   │
└─────────────────────────────┘
```

### 4. Graceful Degradation
- MicroSaaS sem Analytics → Remove tab Analytics do Dashboard
- MicroSaaS sem Badges → Remove seção Badges do Progresso

---

**Última Atualização**: 2025-12-26
