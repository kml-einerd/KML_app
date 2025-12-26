# SIGNUP (CADASTRO)

## 1) Objetivo da Tela
Criar nova conta com consentimentos (LGPD/privacidade) e preparar onboarding.

## 2) Usuário & Contexto
**Usuário**: Visitante novo, **Contexto**: Primeira vez no app, **Frequência**: 1x (único)

## 3) Layout & Hierarquia
```
[Logo/Branding]
"Bem-vindo! Crie sua conta."

[Form]
Nome: [_______]
Email: [_______]
Senha: [_______]

[Checkboxes - Obrigatórios]
☑ Li e aceito os Termos de Serviço
☑ Li e aceito a Política de Privacidade (LGPD)

[Botão: "Criar Conta"]

[Link: "Já tenho conta"]
```

## 4) Elementos & Componentes
- **Inputs**: Nome, Email, Senha (validação forte)
- **Checkboxes**: Termos + Privacidade (obrigatórios)
- **Links**: Termos, Privacidade (abrem modals ou páginas)

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Legal/Compliance → Termos para UGC]

## 5) Ação Primária
"Criar Conta"

## 6) Estados
Loading, Error ("Email já cadastrado"), Success (redireciona ao Onboarding)

## 7) Conteúdo / Microcopy
- "Crie sua conta" (não "Cadastre-se")
- Checkboxes: "Li e aceito..." (obrigatórios)

## 8) Som/Haptics
**SAFE**: `signup_success.mp3`

## 9) Eventos
`signup_attempted`, `signup_success`, `signup_failed`

## 10) Definition of Done
- [ ] Validação de senha forte
- [ ] Checkboxes obrigatórios (bloqueiam submit se não marcados)
- [ ] Redireciona ao Onboarding após sucesso

## 11) Modo Studio / Edições
Todos mantêm

## 12) Mapeamento Back
`POST /api/auth/signup` → retorna `access_token`, flags onboarding

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Entrada → Signup]
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Legal/Compliance → Privacy (LGPD)]

---
