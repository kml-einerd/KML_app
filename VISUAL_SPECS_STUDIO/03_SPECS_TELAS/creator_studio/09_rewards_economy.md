# REWARDS / ECONOMY

## 1) Objetivo da Tela
Configurar economia do app: XP rules, badges, loja.

[fonte: 03 - todas as telas, flows e governança fechados.md → Economia & Recompensas]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer ajustar XP/badges, **Frequência**: 1x na criação, ajustes ocasionais

## 3) Layout & Hierarquia
```
[Header: "Economia & Recompensas"]

[XP Rules]
Presets: [Simples (recomendado) / Avançado ▼]

Missão: 10-20 XP
Aula Interativa: 50-80 XP
Aplicação: 120-200 XP

Multiplicadores:
Streak (7d): 1.1x
Checkpoints 100%: +10 XP fixo

[Badges]
[Lista de Badges]
• "Mestre da Big Idea" - 10 Big Ideas validadas
[Editar] [Criar Novo]

[Loja - Opcional]
☐ Ativar Loja de Recompensas
```

[fonte: 01 - economia.md → Game Designer → XP base por arquétipo]

## 4) Elementos & Componentes
- **Presets**: Dropdown (Simples/Avançado)
- **XP Inputs**: Ranges ou valores fixos
- **Badge List**: Cards editáveis
- **Toggle Loja**: Checkbox

## 5) Ação Primária
"Salvar"

## 6) Estados
- **Success**: "Economia salva"

## 7) Conteúdo / Microcopy
- "Preset Simples (recomendado)" → valores padrão saudáveis
- Aviso: "Bônus agressivos criam ansiedade"

[fonte: 01 - economia.md → Economista LiveOps → Multiplicadores simplificados]

## 8) Som/Haptics
**SAFE**: `save_success.mp3`

## 9) Eventos
`economy_changed`, `badge_created`

## 10) Definition of Done
- [ ] Presets funcionam
- [ ] XP por formato editável
- [ ] Badges criáveis/editáveis
- [ ] Validação previne bônus agressivos

## 11) Modo Studio / Edições
- **MicroSaaS**: Preset fixo (não editável)
- **Standard/Full**: Presets + edição

## 12) Mapeamento Back
`PUT /api/creator/infoapp/:id/economy`

## 13) Rastreabilidade
[fonte: 01 - economia.md → Game Economy]

---
