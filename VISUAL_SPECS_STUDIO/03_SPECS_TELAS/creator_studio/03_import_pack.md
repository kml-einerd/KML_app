# IMPORT PACK (.zip)

## 1) Objetivo da Tela
Upload e validação de Import Pack (.zip) com estrutura: manifest.yaml + tracks.yaml + lessons/*.yaml + missions/*.yaml + applications/*.yaml + srs/vocab.csv + assets/.

[fonte: 04 - modo Studio.md → Modelo de importação em massa]

## 2) Usuário & Contexto
**Usuário**: Creator, **Contexto**: Quer importar conteúdo em volume, **Frequência**: 1-3x por app

## 3) Layout & Hierarquia
```
[Header: "Importar Pacote de Conteúdo"]

[Upload Area - Drag & Drop]
"Arraste o arquivo .zip ou clique para selecionar"

[Após Upload]
✓ manifest.yaml - OK
✓ tracks.yaml - OK
✓ lessons/lesson_001.yaml - OK
⚠️ lessons/lesson_002.yaml - WARNING: Beat 3 sem checkpoint
❌ applications/apply_001.yaml - ERROR: Rubrica ausente

[Botões]
[Corrigir Agora] [Importar Mesmo Assim (se apenas warnings)]
```

[fonte: 04 - modo Studio.md → Fluxo de importação]

## 4) Elementos & Componentes
- **Upload Area**: Drag & drop ou file picker
- **Validation Results**: Lista de arquivos com status (OK/WARNING/ERROR)
- **Bloqueios**: Se ERROR, não pode importar

[fonte: 05 - sistema completo de EdTech.md → Quality Gates → Gate 1: Atividade]

## 5) Ação Primária
"Importar" (se validação OK) ou "Corrigir Agora" (se erros)

## 6) Estados
- **Empty**: Aguardando upload
- **Loading**: Validando estrutura
- **Error**: Erros de validação bloqueiam
- **Warning**: Avisos mas pode importar
- **Success**: "Pacote importado com sucesso"

## 7) Conteúdo / Microcopy
- "Arraste o arquivo .zip"
- Erros: "Beat sem checkpoint bloqueia publicação"
- Avisos: "Recomendamos adicionar exemplo real"

## 8) Som/Haptics
**SAFE**: `upload_success.mp3`, `error_soft.mp3` (se erro)

## 9) Eventos
`pack_uploaded`, `pack_validation_started`, `pack_validation_completed` (propriedades: `errors_count`, `warnings_count`), `pack_imported`

## 10) Definition of Done
- [ ] Upload drag & drop funcional
- [ ] Validação de estrutura (manifest, tracks, lessons, etc.)
- [ ] Erros bloqueiam import
- [ ] Warnings permitem import mas alertam
- [ ] Preview de conteúdo antes de importar

## 11) Modo Studio / Edições
- **MicroSaaS**: Remove (apenas build manual)
- **Standard/Full**: Mantém

## 12) Mapeamento Back
`POST /api/creator/infoapp/:id/import` (payload: multipart file .zip)
Validação server-side:
- Estrutura de arquivos
- Campos obrigatórios (checkpoint por beat, rubrica em aplicação, etc.)

Atalho: Biblioteca de validação YAML (js-yaml)

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Staff Engineer → QA gates validam antes de publish]

## 13) Rastreabilidade
[fonte: 04 - modo Studio.md → Estrutura do pacote → infoapp_pack.zip]
[fonte: 04 - modo Studio.md → Fluxo de importação → Validação]

---
