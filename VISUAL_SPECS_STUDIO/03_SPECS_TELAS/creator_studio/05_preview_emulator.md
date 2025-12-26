# PREVIEW EMULATOR

## 1) Objetivo da Tela
Simular experiência do aluno (Home/Player/Trilha) antes de publicar, com preview por perfil (SAFE/TENSION/STATUS) e dispositivo.

[fonte: 03 - todas as telas, flows e governança fechados.md → Preview → Preview Emulator]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Antes de publicar, quer ver como aluno vê, **Frequência**: 1x antes de cada publish

## 3) Layout & Hierarquia
```
[TopBar]
Preview: [Home/Player/Trilha ▼]
Perfil: [SAFE/TENSION/STATUS ▼]
Dispositivo: [Mobile/Tablet ▼]

[Emulator Frame - Mobile ou Tablet]
┌─────────────┐
│ [Simulação] │
│ (Renderiza) │
│ telas do    │
│ Learner)    │
└─────────────┘

[Botão: "Fechar Preview"]
```

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Versioning + Preview Emulator]

## 4) Elementos & Componentes
- **Dropdown Tela**: Home, Player, Trilha
- **Dropdown Perfil**: SAFE, TENSION, STATUS (muda tokens visuais)
- **Dropdown Dispositivo**: Mobile, Tablet
- **Emulator Frame**: iFrame ou componente que renderiza telas do Learner

## 5) Ação Primária
Navegação dentro do emulator (simula fluxo do aluno)

## 6) Estados
- **Loading**: Carregando preview
- **Success**: Emulator renderizado

## 7) Conteúdo / Microcopy
- "Preview como aluno verá"
- Nota: "Dados de preview (não afeta produção)"

## 8) Som/Haptics
**SAFE**: Sons do Learner tocam no preview (se toggle ativo)

## 9) Eventos
`preview_opened`, `preview_screen_changed`, `preview_profile_changed`

## 10) Definition of Done
- [ ] Emulator renderiza telas do Learner
- [ ] Preview por perfil (SAFE/TENSION/STATUS) funciona
- [ ] Preview por dispositivo (Mobile/Tablet) funciona
- [ ] Navegação no emulator funcional

## 11) Modo Studio / Edições
- **MicroSaaS**: Remove (feature Standard+)
- **Standard/Full**: Mantém

## 12) Mapeamento Back
Preview usa dados do draft (não publicado)
`GET /api/creator/infoapp/:id/preview` → retorna dados para renderizar

Atalho: iFrame ou web view que carrega componentes do Learner em modo preview

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Preview → Preview por perfil]
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Preview Emulator]

---
