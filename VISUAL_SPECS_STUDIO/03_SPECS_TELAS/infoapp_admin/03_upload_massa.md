# InfoApp Admin - Upload em Massa

**App**: InfoApp Admin Panel
**Tela**: Upload em Massa (Bulk Import)
**Versão**: 2.0
**Data**: 2025-12-26
**Mudança**: Movido do Modo Studio para InfoApp Admin Panel

---

## 1. CONTEXTO

**O que é**: Interface para criador importar conteúdo em massa (lessons, beats, checkpoints) via CSV, JSON ou YAML.

**Quando**: Criador quer popular InfoApp rapidamente (ex: importar ebook completo transformado em lessons)

**Usuário**: Criador (dono do InfoApp)

**Cliente disse**: Upload em massa é feito NO InfoApp Admin, não no Modo Studio

---

## 2. ESTRUTURA VISUAL

```
┌────────────────────────────────────────────────────────────────┐
│ [Sidebar]           📥 Upload em Massa                         │
│                                                                │
│                     [CSV] [JSON] [YAML]  (tabs)               │
│                     ─────────────────────────────────────────  │
│                     📄 Importar Lessons via CSV                │
│                                                                │
│                     1. Baixe o template                        │
│                        [📥 Baixar Template CSV]                │
│                                                                │
│                     2. Preencha com seus dados                 │
│                        (abra em Excel/Google Sheets)           │
│                                                                │
│                     3. Faça upload do arquivo                  │
│                        ┌─────────────────────────────────┐    │
│                        │ Arraste arquivo CSV aqui        │    │
│                        │ ou clique para selecionar       │    │
│                        └─────────────────────────────────┘    │
│                        [Selecionar Arquivo]                    │
│                                                                │
│                     4. Validar e importar                      │
│                        (aparece após upload)                   │
│                                                                │
│                     ─────────────────────────────────────────  │
│                     📊 Histórico de Importações                │
│                     - 2025-01-15: 12 lessons (sucesso)         │
│                     - 2025-01-10: 5 lessons (3 erros)          │
└────────────────────────────────────────────────────────────────┘
```

---

## 3. COMPONENTES

### 3.1. Tabs de Formato

**Opções**:
- CSV (padrão, mais simples)
- JSON (mais flexível)
- YAML (mais legível)

**v1**: Apenas CSV (outros formatos em v1.1)

### 3.2. Passo 1: Baixar Template

**Botão**: "Baixar Template CSV"

**Template CSV** (exemplo):
```csv
station,lesson_title,lesson_description,lesson_xp,lesson_coins,beat_order,beat_type,beat_text,beat_image_url,checkpoint_after_beat,checkpoint_type,checkpoint_question,checkpoint_options,checkpoint_correct
Fundamentos,Introdução ao Python,Aprenda o básico,50,10,1,text,"Python é uma linguagem...",,,,,,,
Fundamentos,Introdução ao Python,Aprenda o básico,50,10,2,text,"Python é usado para...",,3,multiple_choice,"Qual é a saída de print(2+2)?","4;22;""4"";erro",4
Fundamentos,Variáveis,Aprenda sobre variáveis,50,10,1,text,"Variáveis armazenam dados...",,,,,,
```

**Colunas**:
- `station`: Nome da estação
- `lesson_title`: Título da lesson
- `lesson_description`: Descrição
- `lesson_xp`: XP reward
- `lesson_coins`: Coins reward
- `beat_order`: Ordem do beat (1, 2, 3...)
- `beat_type`: text, image, video, code
- `beat_text`: Conteúdo do beat (se type=text)
- `beat_image_url`: URL da imagem (se type=image)
- `checkpoint_after_beat`: Número do beat após o qual aparece checkpoint
- `checkpoint_type`: multiple_choice, match, fill_blank
- `checkpoint_question`: Pergunta do checkpoint
- `checkpoint_options`: Opções separadas por `;`
- `checkpoint_correct`: Resposta correta

**Geração de TTS**: Automática após importação (para beats de texto)

### 3.3. Passo 2: Preencher Dados

**Orientação**: Modal ou seção explicativa mostrando como preencher CSV

**Exemplo**:
```
ℹ️  Como preencher o CSV:
- Cada linha é um beat de uma lesson
- Mesma lesson = repetir lesson_title nas linhas
- Checkpoint = preencher colunas checkpoint_*
- Deixe colunas vazias se não aplicável
```

### 3.4. Passo 3: Upload de Arquivo

**Componente**: Drag-and-drop zone

**Funcionalidade**:
- Arraste arquivo CSV
- Ou clique "Selecionar Arquivo" (file picker)
- Validação: Aceita apenas .csv (max 10 MB)

**Após upload**:
- Mostra nome do arquivo e tamanho
- Botão "Remover" (se quiser trocar arquivo)
- Botão "Validar" (processa CSV)

### 3.5. Passo 4: Validação e Importação

**Fluxo**:
1. Clique "Validar"
2. Backend processa CSV e retorna erros (se houver)
3. Mostra preview de o que será importado

**Validação (Preview)**:
```
┌────────────────────────────────────────────────────────────────┐
│ ✅ Validação concluída!                                        │
│                                                                │
│ 📊 Resumo:                                                     │
│   - 3 lessons                                                  │
│   - 12 beats                                                   │
│   - 5 checkpoints                                              │
│                                                                │
│ ⚠️  Avisos:                                                    │
│   - Lesson "Variáveis" já existe (será atualizada)            │
│   - Beat 5 da lesson "Loops" sem texto (será ignorado)        │
│                                                                │
│ ❌ Erros:                                                      │
│   - Linha 10: checkpoint_correct inválido ("cinco" não está   │
│     nas opções)                                                │
│   - Linha 15: beat_type desconhecido ("audio")                │
│                                                                │
│ [Corrigir CSV]  [Importar Mesmo Assim]  [Cancelar]           │
└────────────────────────────────────────────────────────────────┘
```

**Se houver erros**:
- Botão "Corrigir CSV" → Baixa CSV com erros marcados (coluna extra: `error_message`)
- Botão "Importar Mesmo Assim" → Importa linhas sem erro, pula linhas com erro

**Se sem erros**:
- Botão "Importar" → Importa conteúdo
- Loading spinner + progresso (0%, 25%, 50%, 75%, 100%)

**Após importação**:
```
┌────────────────────────────────────────────────────────────────┐
│ ✅ Importação concluída com sucesso!                           │
│                                                                │
│ 📊 Importado:                                                  │
│   - 3 lessons criadas                                          │
│   - 12 beats criados                                           │
│   - 5 checkpoints criados                                      │
│   - 12 áudios TTS gerados (em progresso...)                    │
│                                                                │
│ [Ver Lessons]  [Nova Importação]                              │
└────────────────────────────────────────────────────────────────┘
```

### 3.6. Histórico de Importações

**Lista**: Importações anteriores (últimas 10)

**Informações por importação**:
- Data/hora
- Arquivo (nome)
- Resultado: "12 lessons criadas" ou "3 erros"
- Ação: [Ver Detalhes] (modal com log completo)

**Exemplo de item**:
```
📅 2025-01-15 14:30
📄 curso_python.csv
✅ 12 lessons criadas, 45 beats, 18 checkpoints
[Ver Detalhes]
```

---

## 4. ESTADOS

### 4.1. Empty State (nenhuma importação)
```
┌────────────────────────────────────────┐
│  📥 Nenhuma importação realizada       │
│  Faça upload do seu primeiro CSV!     │
└────────────────────────────────────────┘
```

### 4.2. Loading (processando CSV)
- Spinner + "Validando arquivo... 45%"

### 4.3. Erro (validação falhou)
- Lista de erros por linha
- Opção de baixar CSV com erros marcados

### 4.4. Sucesso
- Toast: "Importação concluída! 🎉"

---

## 5. INTERAÇÕES

### 5.1. Fluxo Completo
1. Clique "Baixar Template CSV"
2. Preenche CSV no Excel/Google Sheets
3. Salva arquivo
4. Arrasta arquivo para dropzone (ou clique "Selecionar")
5. Clique "Validar"
6. Revisa preview
7. Clique "Importar"
8. Aguarda conclusão
9. Clique "Ver Lessons" → Redireciona para Gestão de Conteúdo

### 5.2. Correção de Erros
1. Validação retorna erros
2. Clique "Corrigir CSV" → Baixa CSV com coluna `error_message`
3. Corrige erros no Excel
4. Faz novo upload
5. Valida novamente

### 5.3. Ver Detalhes de Importação Antiga
1. Clique "Ver Detalhes" no histórico
2. Modal mostra log completo da importação
3. Opção de baixar log em .txt

---

## 6. REGRAS DE NEGÓCIO

### 6.1. Lesson Duplicada
- Se lesson com mesmo título já existe na estação, pergunta:
  - "Substituir lesson existente" (sobrescreve)
  - "Criar nova versão" (adiciona sufixo " (2)")
  - "Pular esta lesson" (não importa)

### 6.2. TTS Assíncrono
- Importação cria lessons/beats imediatamente (sem áudio)
- TTS é gerado em background (job assíncrono)
- Notificação: "TTS gerado para 12 beats!" (quando completo)

### 6.3. Limite de Upload
- Max 10 MB por arquivo (v1)
- Max 100 lessons por importação (v1)
- v1.1: Suporta arquivos maiores (chunking)

### 6.4. Validações
- `station`: Precisa existir (ou criar automaticamente?)
- `lesson_title`: Obrigatório
- `beat_type`: Apenas valores válidos (text, image, video, code)
- `checkpoint_correct`: Precisa estar nas `checkpoint_options`

### 6.5. Rollback
- Se importação falhar no meio (erro de DB), fazer rollback completo
- Ou importar tudo em transação (all-or-nothing)

---

## 7. RESPONSIVO

**Desktop**: Layout padrão
**Tablet/Mobile**: Dropzone menor, botões empilhados

---

## 8. ANALYTICS (Tracking)

**Eventos**:
- `admin_bulk_import_started`: Ao fazer upload de arquivo
- `admin_bulk_import_validated`: Ao validar CSV
- `admin_bulk_import_completed`: Ao concluir importação (param: lessons_count, beats_count, errors_count)
- `admin_bulk_template_downloaded`: Ao baixar template

---

## 9. ACESSIBILIDADE

- Dropzone tem `aria-label` "Área de upload de arquivo CSV"
- Screen reader anuncia progresso de importação
- Erros de validação têm `role="alert"`

---

## 10. NOTAS TÉCNICAS

**API Endpoints**:
- `GET /api/admin/import/template`: Baixa template CSV
- `POST /api/admin/import/upload`: Upload de arquivo (retorna upload_id)
- `POST /api/admin/import/validate`: Valida arquivo (param: upload_id)
- `POST /api/admin/import/execute`: Executa importação (param: upload_id)
- `GET /api/admin/import/history`: Lista importações anteriores
- `GET /api/admin/import/:id`: Detalhes de importação específica

**Processamento**:
```python
# Pseudocódigo
def process_csv(file):
    rows = parse_csv(file)
    lessons_map = {}  # {lesson_title: lesson_obj}
    errors = []

    for i, row in enumerate(rows):
        try:
            # Validar linha
            validate_row(row)

            # Criar/atualizar lesson
            if row['lesson_title'] not in lessons_map:
                lesson = Lesson.create(
                    title=row['lesson_title'],
                    description=row['lesson_description'],
                    station=row['station'],
                    xp_reward=row['lesson_xp'],
                    coins_reward=row['lesson_coins']
                )
                lessons_map[row['lesson_title']] = lesson
            else:
                lesson = lessons_map[row['lesson_title']]

            # Criar beat
            beat = Beat.create(
                lesson_id=lesson.id,
                order=row['beat_order'],
                type=row['beat_type'],
                content={'text': row['beat_text'], 'image_url': row['beat_image_url']}
            )

            # Gerar TTS (assíncrono)
            if row['beat_type'] == 'text':
                queue_tts_generation(beat.id, row['beat_text'])

            # Criar checkpoint (se houver)
            if row['checkpoint_question']:
                Checkpoint.create(
                    lesson_id=lesson.id,
                    after_beat=row['checkpoint_after_beat'],
                    type=row['checkpoint_type'],
                    question=row['checkpoint_question'],
                    options=row['checkpoint_options'].split(';'),
                    correct=row['checkpoint_correct']
                )

        except ValidationError as e:
            errors.append({'line': i+1, 'error': str(e)})

    return {
        'lessons_created': len(lessons_map),
        'beats_created': Beat.count(),
        'checkpoints_created': Checkpoint.count(),
        'errors': errors
    }
```

---

## 11. PERGUNTAS PARA O CLIENTE

1. **Formato prioritário**: CSV suficiente para v1 ou precisa JSON/YAML também?
2. **Estações**: Se estação não existe, criar automaticamente ou dar erro?
3. **Lesson duplicada**: Comportamento padrão? (substituir, versão nova, pular)
4. **Limit de upload**: 100 lessons/importação ok? Ou precisa mais?
5. **Transformação de conteúdo**: Cliente falou em "transformar ebook em SaaS". Haverá ferramenta de IA para converter PDF → CSV automaticamente? (v1.1?)

---

**Status**: DRAFT
**Próxima revisão**: [Data]
