# PUBLISH

## 1) Objetivo da Tela
Publicar versão do InfoApp (após QA OK) com changelog, criando versão v1, v2, etc.

[fonte: 03 - todas as telas, flows e governança fechados.md → Publish & Versões → Publish]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Após QA OK, **Frequência**: 1x por versão

## 3) Layout & Hierarquia
```
[Header: "Publicar Versão"]

Versão Atual: v1.0 (publicada em 01/12/2025)
Nova Versão: v1.1

[Changelog]
"O que mudou nesta versão?"
[Textarea obrigatório]

[Marketplace - Checkbox]
☐ Listar no Marketplace Público
   (Requer aceitar Termos de Marketplace)

[Review Final]
✓ QA Checklist: OK
✓ Changelog: Preenchido
☐ Termos aceitos (se marketplace)

[Botão Primário: "Publicar v1.1"]
```

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Versioning + Preview]

## 4) Elementos & Componentes
- **Versioning Info**: Versão atual vs nova
- **Changelog**: Textarea obrigatório
- **Marketplace Checkbox**: Opcional (com termos)
- **Review Final**: Checklist visual

## 5) Ação Primária
"Publicar v[X]"

## 6) Estados
- **Loading**: Publicando...
- **Error**: "Falha ao publicar"
- **Success**: "Versão v1.1 publicada com sucesso" + redireciona a Versioning

## 7) Conteúdo / Microcopy
- "O que mudou nesta versão?" (obrigatório)
- Marketplace: "Listar no Marketplace Público" + link "Termos de Marketplace"
- Sucesso: "Versão publicada! Seus alunos já podem acessar."

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Legal/Compliance → Termos para marketplace]

## 8) Som/Haptics
**STATUS**: `publish_success.mp3` (celebração moderada), confetti se primeira publicação

## 9) Eventos
`publish_started`, `publish_success`, `marketplace_listed` (se checkbox marcado)

## 10) Definition of Done
- [ ] Changelog obrigatório
- [ ] Versioning automático (v1.0 → v1.1)
- [ ] Marketplace checkbox com termos
- [ ] Publicação cria nova versão
- [ ] Redireciona a Versioning após sucesso

## 11) Modo Studio / Edições
- **MicroSaaS**: Sem versioning (apenas v1.0)
- **Standard/Full**: Versioning completo

## 12) Mapeamento Back
`POST /api/creator/infoapp/:id/publish` (payload: `changelog`, `list_marketplace`)
Cria nova versão no banco, ativa para alunos

## 13) Rastreabilidade
[fonte: 03 - todas as telas, flows e governança fechados.md → Publish & Versões]
[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → Versioning]

---
