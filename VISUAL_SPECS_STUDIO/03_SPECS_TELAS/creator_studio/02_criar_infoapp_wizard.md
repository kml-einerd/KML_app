# CRIAR INFOAPP (WIZARD)

## 1) Objetivo da Tela
Wizard guiado (4-5 passos) para criar novo InfoApp com nome, nicho, idioma, branding e objetivos de aprendizagem.

[fonte: 03 - todas as telas, flows e governança fechados.md → Creator Studio → Criar InfoApp → Wizard]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer criar novo app, **Frequência**: 1x por app novo

## 3) Layout & Hierarquia (Passo 1 de 4)
```
[Progress: 1/4]

"Nome e Nicho do InfoApp"

Nome: [_______________________]
Nicho: [Dropdown: Negócios/Saúde/Criativo/Outro]

[Botão: "Próximo"]
[Link: "Cancelar"]
```

**Passos do Wizard**:
1. Nome + Nicho
2. Idioma + Público-alvo
3. Branding (cor/logo)
4. Objetivos de Aprendizagem
5. Review + Criar

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Content Ops / Editorial Lead → Objetivos obrigatórios]

## 4) Elementos & Componentes
- **Steps Indicator**: 1/4, 2/4, etc.
- **Form Fields**: Input text, dropdowns, color picker, file upload (logo)
- **Botões**: "Próximo", "Voltar", "Criar"

## 5) Ação Primária
"Próximo" (ou "Criar" no passo final)

## 6) Estados
- **Loading**: Criando app (passo final)
- **Error**: "Nome já existe", "Campo obrigatório vazio"
- **Success**: Redireciona ao Dashboard (app draft criado)

## 7) Conteúdo / Microcopy
- Passo 1: "Nome e Nicho"
- Passo 2: "Idioma e Público"
- Passo 3: "Branding Visual"
- Passo 4: "Objetivos de Aprendizagem" → "O que o aluno será capaz de fazer ao final?"

**Princípio**: Criador não "monta app", escolhe formato oficial.

[fonte: 01 - economia.md → Arq. de Produto → Criador escolhe formato]

## 8) Som/Haptics
**SAFE**: `step_complete.mp3` ao avançar passo

## 9) Eventos
`wizard_step_completed` (step: 1-4), `infoapp_created`

## 10) Definition of Done
- [ ] Wizard 4-5 passos funcional
- [ ] Validação de campos obrigatórios
- [ ] Campo "Objetivos de Aprendizagem" obrigatório
- [ ] Redireciona ao Dashboard após criar

## 11) Modo Studio / Edições
Todos mantêm (criação obrigatória)

## 12) Mapeamento Back
`POST /api/creator/infoapp/create` (payload: `name`, `niche`, `language`, `branding`, `learning_outcomes`)

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Creator Studio → Criar InfoApp]
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Editorial Lead → Objetivos obrigatórios]

---
