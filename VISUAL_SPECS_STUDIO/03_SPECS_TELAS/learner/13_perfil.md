# PERFIL

## 1) Objetivo da Tela
Exibir informações do aluno e dar acesso a Settings/Ajuda/Sair.

## 2) Usuário & Contexto
**Usuário**: Learner, **Contexto**: Quer ver perfil ou acessar configurações, **Frequência**: 1x por semana

## 3) Layout & Hierarquia
```
[Avatar + Nome]
João Silva
Nível 5 • 476 XP

[Menu]
⚙️ Configurações
❓ Ajuda
📧 Contato
🚪 Sair
```

## 4) Elementos & Componentes
- Avatar, Nome, XP
- Menu com links

## 5) Ação Primária
"Configurações"

## 6) Estados
Success

## 7) Conteúdo / Microcopy
"Configurações", "Ajuda", "Sair"

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`

## 9) Eventos
`profile_viewed`, `settings_opened`, `logout`

## 10) Definition of Done
- [ ] Avatar + Nome exibidos
- [ ] Menu funcional
- [ ] Sair desloga corretamente

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`GET /api/learner/profile`, `POST /api/auth/logout`

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Conta → Perfil]

---
