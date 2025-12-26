# CONFIGURAÇÕES

## 1) Objetivo da Tela
Controle total: som/haptics/motion/notificações/privacidade.

[fonte: 07 - alinhamento.md → Accessibility → Controles para som/haptics/motion]

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Ajustar preferências, **Frequência**: 1x por mês (ou ao precisar)

## 3) Layout & Hierarquia
```
[Seções]

**Acessibilidade**
🔊 Som: [ON/OFF]
📳 Haptics: [ON/OFF]
🎬 Motion: [Reduzir/Normal]
📝 Legendas: [ON/OFF]

**Notificações**
🔔 Streak em risco: [ON/OFF]
📬 Novas missões: [ON/OFF]

**Privacidade**
🔒 Dados pessoais
🗑️ Excluir conta

[Botão: "Salvar"]
```

## 4) Elementos & Componentes
- Toggle switches
- Links para políticas

[fonte: 06_SISTEMA_SOM/regras_acessibilidade.md]

## 5) Ação Primária
"Salvar"

## 6) Estados
Loading, Success ("Configurações salvas")

## 7) Conteúdo / Microcopy
"Som", "Haptics", "Reduzir motion", "Legendas"

## 8) Som/Haptics
**SAFE**: `settings_saved.mp3`

## 9) Eventos
`settings_changed` (propriedades: `setting`, `new_value`)

## 10) Definition of Done
- [ ] Toggles funcionais
- [ ] Preferências salvas persistem
- [ ] Reduce motion/sound funcionam globalmente

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`PUT /api/learner/preferences`

## 13) Rastreabilidade
[fonte: 07 - alinhamento.md → Accessibility → Controles obrigatórios]
[fonte: 03 - todas as telas, flows e governança fechados.md → Conta → Configurações]

---
