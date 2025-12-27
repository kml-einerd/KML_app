# Boas Práticas: Importação de Conteúdo

**Versão**: 1.0
**Data**: 2025-12-27
**Público**: Criadores de conteúdo (pedagogia + estrutura)
**Objetivo**: Garantir qualidade pedagógica e técnica nos InfoApps

---

## 1. PEDAGOGIA (EDTECH)

### 1.1. Princípio: Aprendizagem Ativa

**Cliente disse**: "100% manual (admin escreve tudo) - é importante ter um guia para os padrões de importação"

**Regra de Ouro**: Todo beat DEVE ter checkpoint (processamento ativo, não consumo passivo)

❌ **RUIM** (consumo passivo):
```yaml
- beat_number: 1
  narration: "Big Idea é importante para copywriting."
  checkpoint: null  # SEM checkpoint = rejeitado na validação
```

✅ **BOM** (aprendizagem ativa):
```yaml
- beat_number: 1
  narration: "Big Idea é importante para copywriting."
  checkpoint:
    type: "QUIZ"
    prompt: "Por que Big Idea é importante?"
    options: [...]
```

---

### 1.2. Duração dos Beats

**Regra**: 45-90 segundos por beat (8-12 beats por aula)

**Razão**: Atenção sustentada + processamento profundo

❌ **RUIM** (muito longo):
```yaml
narration: |
  Este texto tem 500 palavras e leva 5 minutos para narrar.
  O aluno vai perder atenção e não vai processar...
  [continua por muito tempo]
```
**Duração estimada**: ~5 min (muito longo)

✅ **BOM** (conciso):
```yaml
narration: |
  Big Idea conecta produto ao desejo do cliente.
  Exemplo: "A dieta que permite comer gordura".
```
**Duração estimada**: ~60 seg (ideal)

**Ferramenta**: Contar caracteres
- Máximo: 300 caracteres por beat
- TTS médio: 150 caracteres/min → 300 caracteres = ~120 seg

---

### 1.3. Checkpoints Construtivos

**Regra**: Feedback deve ser educativo, não punitivo

❌ **RUIM** (punitivo):
```yaml
feedback: "Errado! Você não sabe nada."
```

✅ **BOM** (educativo):
```yaml
feedback: |
  Não exatamente. Big Idea não é apenas uma afirmação genérica.
  Ela precisa ter novidade, especificidade e promessa.
  Tente novamente pensando nesses 3 elementos.
```

---

### 1.4. Objetivos de Aprendizagem Claros

**Regra**: Todo track/lesson deve declarar "o que você será capaz de fazer"

❌ **RUIM** (vago):
```yaml
objectives:
  - "Aprender sobre Big Ideas"
```

✅ **BOM** (específico, mensurável):
```yaml
objectives:
  - "Criar Big Ideas usando framework de 3 elementos"
  - "Avaliar Big Ideas de campanhas reais"
  - "Aplicar Big Ideas em seu próprio negócio"
```

---

### 1.5. Aplicações (Transferência para Mundo Real)

**Regra**: Todo track deve ter 1 aplicação (atividade prática)

❌ **RUIM** (apenas teoria):
```yaml
tracks_data:
  - track_id: "track_001"
    lessons:
      - lesson_001
      - lesson_002
    application:
      enabled: false  # SEM aplicação = aprendizagem superficial
```

✅ **BOM** (teoria + prática):
```yaml
tracks_data:
  - track_id: "track_001"
    lessons:
      - lesson_001
      - lesson_002
    application:
      enabled: true
      application_id: "app_001"  # Atividade prática
```

---

## 2. ESTRUTURA TÉCNICA

### 2.1. Nomeação de IDs

**Convenção**:
- Prefixo claro: `track_`, `lesson_`, `mission_`, `app_`
- Números com zero à esquerda: `001`, `002`, ..., `010`, `099`, `100`
- Minúsculas, sem espaços

❌ **RUIM**:
- `Track 1` (espaço, maiúscula)
- `track-1` (hífen, sem zero à esquerda)
- `trilha_001` (nome em português)

✅ **BOM**:
- `track_001`
- `track_002`
- `track_010`

---

### 2.2. Referências Consistentes

**Regra**: IDs devem ser únicos e consistentes em todo o arquivo

❌ **RUIM** (inconsistente):
```yaml
tracks:
  - track_001  # Declarado aqui

tracks_data:
  - track_id: "track_1"  # ID diferente! Erro de referência
```

✅ **BOM** (consistente):
```yaml
tracks:
  - track_001

tracks_data:
  - track_id: "track_001"  # Mesmo ID
```

---

### 2.3. Índices de Arrays (0-indexed)

**Regra**: `correct_index` em QUIZ é 0-indexed (primeira opção = 0)

❌ **RUIM**:
```yaml
options:
  - text: "Opção A"
  - text: "Opção B (correta)"
  - text: "Opção C"
correct_index: 2  # ERRO! Opção B é índice 1, não 2
```

✅ **BOM**:
```yaml
options:
  - text: "Opção A"
  - text: "Opção B (correta)"
  - text: "Opção C"
correct_index: 1  # Correto (0=A, 1=B, 2=C)
```

---

### 2.4. Campos Obrigatórios

**Regra**: Sempre preencha campos obrigatórios (validação vai rejeitar se faltar)

**Campos obrigatórios por entidade**:

**Manifest**:
- `manifest_version`
- `infoapp_id`
- `title`
- `language`
- `tracks`

**Track**:
- `track_id`
- `order`
- `title`
- `lessons`

**Lesson**:
- `lesson_id`
- `track_id`
- `order`
- `title`
- `beats`

**Beat**:
- `beat_number`
- `narration`
- `checkpoint`

**Checkpoint**:
- `type`
- `prompt`

---

### 2.5. Tipos de Dados

**Regra**: Respeite tipos de dados (string, number, boolean, array, object)

❌ **RUIM**:
```yaml
order: "1"  # String (aspas)
```

✅ **BOM**:
```yaml
order: 1  # Number (sem aspas)
```

❌ **RUIM**:
```yaml
enabled: "true"  # String
```

✅ **BOM**:
```yaml
enabled: true  # Boolean
```

---

## 3. QUALIDADE DE CONTEÚDO

### 3.1. Exemplos Reais

**Regra**: Sempre use exemplos concretos, não abstrações

❌ **RUIM** (abstrato):
```yaml
narration: "Big Idea é um conceito importante."
```

✅ **BOM** (exemplo real):
```yaml
narration: |
  Exemplo: "A dieta que permite comer gordura e perder peso".
  Esta Big Idea da Atkins quebrou paradigma e criou mercado bilionário.
```

---

### 3.2. Linguagem Clara

**Regra**: Evite jargões sem explicação

❌ **RUIM** (jargão sem contexto):
```yaml
narration: "Use framework de value prop para criar USP."
```

✅ **BOM** (explica jargões):
```yaml
narration: |
  Use framework de proposta de valor (o que torna seu produto único)
  para criar sua USP (Unique Selling Proposition - diferencial competitivo).
```

---

### 3.3. Progressão Lógica

**Regra**: Sequência de beats deve seguir progressão lógica

❌ **RUIM** (ordem confusa):
```yaml
beats:
  - beat_number: 1
    narration: "Big Idea tem 3 elementos."  # Ainda não explicou o que é Big Idea
  - beat_number: 2
    narration: "Big Idea é importante."  # Deveria vir antes
```

✅ **BOM** (progressão lógica):
```yaml
beats:
  - beat_number: 1
    narration: "Big Idea é a ideia central que conecta produto ao desejo."  # Define
  - beat_number: 2
    narration: "Exemplo: dieta Atkins."  # Exemplifica
  - beat_number: 3
    narration: "Big Idea tem 3 elementos: Novidade, Especificidade, Promessa."  # Detalha
```

---

## 4. GAMIFICAÇÃO EQUILIBRADA

### 4.1. Economia de Coins

**Regra**: Recompensas devem ser balanceadas (não inflação)

**Valores recomendados**:
- Completar beat: 1-2 coins
- Completar lesson: 10-20 coins
- Completar track: 50-100 coins
- Perfect score bonus: +10-30 coins

❌ **RUIM** (inflação):
```yaml
rewards:
  coins_base: 1000  # Muito alto! Aluno fica "rico" rápido
```

✅ **BOM** (balanceado):
```yaml
rewards:
  coins_base: 10
  coins_perfect_score: 20
```

---

### 4.2. Badges Significativos

**Regra**: Badges devem marcar conquistas reais, não triviais

❌ **RUIM** (trivial):
```yaml
badge: "badge_primeiro_login"  # Ganhar badge por logar?
```

✅ **BOM** (conquista real):
```yaml
badge: "badge_big_idea_master"  # Ganhar após completar track inteiro
```

---

## 5. ASSETS (IMAGENS/ÁUDIOS)

### 5.1. Organização de Pasta

**Estrutura recomendada**:
```
assets/
├── images/
│   ├── cover.png           # Capa do InfoApp
│   ├── logo.png            # Logo
│   ├── track_001_cover.png # Capa do track
│   └── products/
│       └── ebook.png       # Produto da loja
└── audios/
    └── intro.mp3           # Áudio customizado (opcional)
```

---

### 5.2. Formatos e Tamanhos

**Imagens**:
- Formato: PNG (transparência) ou JPG (fotos)
- Tamanho máximo: 2 MB
- Resolução: 1200x630px (cover), 512x512px (logo/ícones)

**Áudios** (opcional, TTS é automático):
- Formato: MP3
- Bitrate: 128 kbps
- Tamanho máximo: 5 MB

---

### 5.3. Nomeação de Assets

❌ **RUIM**:
- `IMG_1234.jpg` (sem contexto)
- `capa do módulo 1.png` (espaços, acentos)

✅ **BOM**:
- `track_001_cover.png`
- `product_ebook_big_ideas.png`

---

## 6. VALIDAÇÃO ANTES DE IMPORTAR

### 6.1. Checklist Pré-Importação

Antes de importar, verifique:

- [ ] **Sintaxe YAML válida** (https://www.yamllint.com/)
- [ ] **IDs únicos e consistentes**
- [ ] **Todos os beats têm checkpoint**
- [ ] **Campos obrigatórios preenchidos**
- [ ] **Referências corretas** (track_id, lesson_id, etc.)
- [ ] **Assets existem** (imagens/áudios referenciados)
- [ ] **Texto de narração < 300 caracteres por beat**
- [ ] **Progressão lógica de beats**
- [ ] **Exemplos concretos (não abstrações)**
- [ ] **Objetivos de aprendizagem claros**

---

### 6.2. Ferramentas de Validação

**Online**:
- YAML Lint: https://www.yamllint.com/
- JSON Validator: https://jsonlint.com/

**Local** (Python):
```bash
# Instalar PyYAML
pip install pyyaml

# Validar YAML
python -c "import yaml; yaml.safe_load(open('manifest.yaml'))"
```

**Local** (Node.js):
```bash
# Instalar js-yaml
npm install js-yaml

# Validar YAML
node -e "const yaml = require('js-yaml'); yaml.load(require('fs').readFileSync('manifest.yaml', 'utf8'))"
```

---

## 7. EXEMPLOS DE ERROS COMUNS

### Erro 1: Indentação Incorreta

❌ **RUIM**:
```yaml
beats:
- beat_number: 1
 narration: "Texto"  # Indentação errada
```

✅ **BOM**:
```yaml
beats:
  - beat_number: 1
    narration: "Texto"  # 2 espaços de indentação
```

---

### Erro 2: Aspas em Campos Numéricos

❌ **RUIM**:
```yaml
order: "1"  # String, deveria ser number
```

✅ **BOM**:
```yaml
order: 1
```

---

### Erro 3: Referência de ID Errada

❌ **RUIM**:
```yaml
tracks:
  - track_001

tracks_data:
  - track_id: "track_1"  # ID diferente!
```

✅ **BOM**:
```yaml
tracks:
  - track_001

tracks_data:
  - track_id: "track_001"  # Mesmo ID
```

---

### Erro 4: Beat sem Checkpoint

❌ **RUIM** (vai ser REJEITADO na validação):
```yaml
beats:
  - beat_number: 1
    narration: "Texto"
    checkpoint: null  # ERRO! Checkpoint obrigatório
```

✅ **BOM**:
```yaml
beats:
  - beat_number: 1
    narration: "Texto"
    checkpoint:
      type: "QUIZ"
      prompt: "Pergunta?"
      options: [...]
      correct_index: 0
```

---

## 8. RECURSOS ADICIONAIS

### Documentação Completa

- **Estrutura YAML**: `/18_GUIA_IMPORTACAO/estrutura_yaml.md`
- **Estrutura JSON**: `/18_GUIA_IMPORTACAO/estrutura_json.md`
- **Exemplo Básico**: `/18_GUIA_IMPORTACAO/exemplos_completos/exemplo_basico.yaml`
- **Schema de Validação**: `/18_GUIA_IMPORTACAO/schema_validacao.json`

### Suporte

- **Fórum**: [link do fórum]
- **Discord**: [link do Discord]
- **Email**: suporte@plataforma.com

---

**Criado por**: EdTech + Content Ops + Tech Writer
**Revisado por**: [Aguardando feedback de criadores]
