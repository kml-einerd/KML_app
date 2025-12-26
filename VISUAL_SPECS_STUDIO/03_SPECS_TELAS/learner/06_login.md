# LOGIN

## 1) Objetivo da Tela
Autenticar usuário de forma rápida e segura (email/senha + social login opcional).

## 2) Usuário & Contexto
**Usuário**: Visitante ou aluno retornando, **Contexto**: Acesso ao app, **Frequência**: 1x por sessão (ou menos com auto-login)

## 3) Layout & Hierarquia
```
[Logo/Branding]

[Form]
Email: [_______]
Senha: [_______]
[Esqueci senha]

[Botão: "Entrar"]

[Divider: "ou"]

[Login Social]
[Continuar com Google]
[Continuar com Apple]

[Link: "Criar conta"]
```

## 4) Elementos & Componentes
- **Input Email**: validação de formato
- **Input Senha**: type=password, toggle show/hide
- **Botão Entrar**: primário, grande
- **Social Login**: Google, Apple (SSO opcional)

[fonte: 03 - todas as telas, flows e governança fechados.md → Entrada → Login social + SSO opcional]

## 5) Ação Primária
"Entrar"

## 6) Estados
Loading (autenticando), Error ("Email ou senha incorretos"), Success (redireciona à Home)

## 7) Conteúdo / Microcopy
- Botão: "Entrar" (não "Login")
- Link: "Esqueci minha senha"
- Link: "Criar conta"

## 8) Som/Haptics
**SAFE**: `ambient_safe.mp3`, `login_success.mp3`

## 9) Eventos
`login_attempted`, `login_success`, `login_failed`, `social_login_used`

## 10) Definition of Done
- [ ] Form valida email/senha
- [ ] Social login funcional (Google/Apple)
- [ ] Erro exibe mensagem clara
- [ ] Redireciona à Home após sucesso

## 11) Modo Studio / Edições
Todos mantêm (entrada obrigatória)

## 12) Mapeamento Back
`POST /api/auth/login` (payload: `email`, `password`) → retorna `access_token`, `refresh_token`
OAuth para social login (Google/Apple)

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Entrada → Login]

---
