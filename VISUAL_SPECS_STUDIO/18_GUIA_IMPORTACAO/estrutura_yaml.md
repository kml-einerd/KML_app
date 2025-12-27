# Guia de Importação: Estrutura YAML

**Versão**: 1.0
**Data**: 2025-12-27
**Público**: Criadores de conteúdo (InfoApps)
**Objetivo**: Documentar estrutura de arquivos YAML para importação em massa

---

## VISÃO GERAL

### Por que YAML?

**Cliente disse**: "Acho que pode ser só YAML e JSON pra padronizar e ter qualidade no upload massivo"

**Vantagens do YAML**:
- ✅ Mais legível que JSON (sem chaves/vírgulas excessivas)
- ✅ Suporta comentários (#)
- ✅ Fácil de editar manualmente (VS Code, Sublime, qualquer editor)
- ✅ Conversível para JSON automaticamente

---

## ESTRUTURA DE ARQUIVOS

### Pacote de Importação (.zip)

```
infoapp_pack.zip
├── manifest.yaml           # Metadados do InfoApp
├── tracks/
│   ├── track_001.yaml      # Módulo 1
│   ├── track_002.yaml      # Módulo 2
│   └── track_003.yaml      # Módulo 3
├── lessons/
│   ├── lesson_001.yaml     # Aula 1
│   ├── lesson_002.yaml     # Aula 2
│   └── lesson_003.yaml     # Aula 3
├── missions/
│   ├── mission_001.yaml    # Missão diária 1
│   └── mission_002.yaml    # Missão diária 2
├── applications/
│   ├── app_001.yaml        # Atividade interativa 1
│   └── app_002.yaml        # Atividade interativa 2
├── srs/
│   └── vocab.yaml          # Vocabulário para review SRS
└── assets/
    ├── images/
    │   └── cover.png       # Imagem de capa
    └── audios/
        └── intro.mp3       # Áudios customizados (opcional)
```

---

## ARQUIVO: manifest.yaml

**Descrição**: Metadados gerais do InfoApp

### Estrutura

```yaml
# ===== METADADOS GERAIS =====
manifest_version: "1.0"
infoapp_id: "copy_big_idea_001"
title: "Copy: Big Idea"
subtitle: "Aprenda a escrever grandes ideias que vendem"
description: |
  Descubra como criar Big Ideas irresistíveis para seus produtos,
  usando frameworks provados do mundo do copywriting.

# ===== CONFIGURAÇÕES =====
language: "pt-BR"  # Idioma principal (pt-BR, en-US, es-ES)
level: "beginner"  # Nível: beginner, intermediate, advanced
category: "marketing"  # Categoria: marketing, tech, health, business, etc.

# ===== BRANDING =====
branding:
  logo: "assets/images/logo.png"
  cover: "assets/images/cover.png"
  primary_color: "#6366f1"  # Cor principal (hex)
  secondary_color: "#8b5cf6"  # Cor secundária

# ===== OBJETIVOS DE APRENDIZAGEM =====
learning_objectives:
  - "Criar Big Ideas originais usando 5 frameworks"
  - "Validar Big Ideas com teste de clareza"
  - "Aplicar Big Ideas em campanhas reais"

# ===== ESTRUTURA =====
tracks:
  - track_001  # Referência ao arquivo tracks/track_001.yaml
  - track_002
  - track_003

# ===== GAMIFICAÇÃO (OPCIONAL) =====
gamification:
  daily_goal_default: 50  # XP padrão por dia
  streak_bonus_enabled: true
  leagues_enabled: true

# ===== LOJA DE RECOMPENSAS (OPCIONAL) =====
store:
  enabled: true
  products:
    - id: "prod_001"
      name: "Ebook: 100 Big Ideas"
      price_coins: 100
      type: "digital"
      image: "assets/images/ebook.png"
```

### Campos Obrigatórios

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `manifest_version` | String | Versão do schema (sempre "1.0") |
| `infoapp_id` | String | ID único (slug, minúsculas, sem espaços) |
| `title` | String | Nome do InfoApp |
| `language` | String | Idioma (pt-BR, en-US, es-ES) |
| `tracks` | Array | Lista de IDs dos tracks |

---

## ARQUIVO: tracks/track_XXX.yaml

**Descrição**: Módulo/estação do InfoApp (agrupa lessons)

### Estrutura

```yaml
# ===== METADADOS DO TRACK =====
track_id: "track_001"
order: 1
title: "Fundamentos da Big Idea"
subtitle: "O que é e por que importa"
description: |
  Neste módulo você vai entender o conceito de Big Idea
  e por que ela é essencial para qualquer campanha de sucesso.

# ===== ÍCONE/VISUAL =====
icon: "🚀"  # Emoji ou URL de ícone
cover_image: "assets/images/track_001_cover.png"

# ===== LESSONS DESTE TRACK =====
lessons:
  - lesson_001
  - lesson_002
  - lesson_003

# ===== APLICAÇÃO (ATIVIDADE FINAL) =====
application:
  enabled: true
  application_id: "app_001"  # Referência a applications/app_001.yaml

# ===== PRÉ-REQUISITOS (OPCIONAL) =====
prerequisites:
  - track_id: null  # Null = primeiro track, sem pré-requisito
  # - track_id: "track_000"  # Se tivesse pré-requisito

# ===== DESBLOQUEIO =====
unlock_conditions:
  type: "sequential"  # Sequencial (track anterior) ou "unlocked" (desbloqueado desde o início)
  required_track: null  # null = primeiro track

# ===== RECOMPENSAS =====
rewards:
  coins: 50  # Coins ao completar track
  badge: "badge_big_idea_basics"  # Badge especial
```

---

## ARQUIVO: lessons/lesson_XXX.yaml

**Descrição**: Aula Interativa (formato principal do InfoApp)

### Estrutura

```yaml
# ===== METADADOS DA LESSON =====
lesson_id: "lesson_001"
track_id: "track_001"  # Pertence ao track X
order: 1
title: "O que é Big Idea?"
subtitle: "Definição e importância"

# ===== DURAÇÃO ESTIMADA =====
estimated_duration_minutes: 8  # Tempo estimado (para planejamento do aluno)

# ===== OBJETIVOS ESPECÍFICOS =====
objectives:
  - "Definir Big Idea com suas próprias palavras"
  - "Identificar Big Ideas em campanhas famosas"

# ===== BEATS (SEQUÊNCIA DE APRENDIZADO) =====
beats:
  - beat_number: 1
    narration: |
      Big Idea é a ideia central, irresistível,
      que conecta seu produto ao desejo do cliente.
    checkpoint:
      type: "QUIZ"
      prompt: "Qual destas é uma Big Idea?"
      options:
        - text: "Produto X é o melhor do mercado"
          feedback: "Isso é uma afirmação genérica, não uma Big Idea."
        - text: "Descubra o segredo que CEOs usam para dobrar produtividade"
          feedback: "Correto! Esta é uma Big Idea: específica, curiosa, promete transformação."
        - text: "Compre agora com 50% off"
          feedback: "Isso é uma oferta, não uma Big Idea."
      correct_index: 1  # Índice da opção correta (0-indexed)
    audio_url: null  # null = gerar via TTS automaticamente
    visual_cue: "highlight"  # Destacar texto importante

  - beat_number: 2
    narration: |
      Exemplo clássico: "A dieta que permite comer gordura e perder peso".
      Esta Big Idea quebrou o padrão de "comer menos" e criou um novo mercado.
    checkpoint:
      type: "REFLECTION"
      prompt: "Pense em uma Big Idea de um produto que você admira. O que torna ela poderosa?"
      min_characters: 50  # Mínimo de caracteres na resposta
    audio_url: null
    visual_cue: "example"

  - beat_number: 3
    narration: |
      Uma Big Idea eficaz tem 3 elementos: Novidade, Especificidade, Promessa.
    checkpoint:
      type: "MATCH"
      prompt: "Relacione cada elemento com sua definição:"
      pairs:
        - left: "Novidade"
          right: "Algo que o público nunca viu dessa forma"
        - left: "Especificidade"
          right: "Detalhe concreto, não genérico"
        - left: "Promessa"
          right: "Transformação clara e desejável"
    audio_url: null
    visual_cue: "framework"

# ===== RECOMPENSAS =====
rewards:
  coins_base: 10
  coins_perfect_score: 20  # Total se 100% acertos
```

### Tipos de Checkpoint

| Tipo | Descrição | Campos |
|------|-----------|--------|
| `QUIZ` | Múltipla escolha | `options`, `correct_index` |
| `REFLECTION` | Resposta aberta | `min_characters` |
| `MATCH` | Relacionar colunas | `pairs` (left/right) |
| `TRUE_FALSE` | Verdadeiro ou falso | `statement`, `correct` |
| `FILL_BLANK` | Preencher lacuna | `text_with_blanks`, `correct_answers` |

---

## ARQUIVO: missions/mission_XXX.yaml

**Descrição**: Missão diária (formato rápido, 2-3 min)

### Estrutura

```yaml
# ===== METADADOS DA MISSÃO =====
mission_id: "mission_001"
title: "Identifique a Big Idea"
description: "Analise uma campanha famosa e identifique sua Big Idea"

# ===== CONTEÚDO =====
challenge:
  type: "ANALYSIS"
  prompt: |
    Assista este anúncio da Apple (1984):
    [Link do vídeo]

    Qual é a Big Idea deste anúncio?
  options:
    - "Apple vende computadores melhores"
    - "Pessoas que pensam diferente mudam o mundo"
    - "Computadores são para todos"
  correct_index: 1

# ===== RECOMPENSAS =====
rewards:
  coins: 5
  first_completion_bonus: 10  # Bonus na primeira vez
```

---

## ARQUIVO: applications/app_XXX.yaml

**Descrição**: Atividade Interativa (aplicação de conhecimento)

### Estrutura

```yaml
# ===== METADADOS =====
application_id: "app_001"
track_id: "track_001"
title: "Crie sua primeira Big Idea"
description: |
  Aplique o framework de Big Ideas para criar
  uma ideia original para um produto à sua escolha.

# ===== TIPO DE ATIVIDADE =====
type: "PROJECT"  # Tipos: PROJECT, QUIZ_ADVANCED, SIMULATION, SCENARIO

# ===== INSTRUÇÕES =====
instructions: |
  1. Escolha um produto (pode ser fictício)
  2. Defina o público-alvo
  3. Crie uma Big Idea usando os 3 elementos (Novidade, Especificidade, Promessa)
  4. Escreva um parágrafo explicando sua Big Idea

# ===== CRITÉRIOS DE AVALIAÇÃO (CHECKLIST) =====
checklist:
  - criteria: "Big Idea tem elemento de novidade"
    description: "Algo único ou ângulo novo"
  - criteria: "Big Idea é específica (não genérica)"
    description: "Detalhe concreto, não vago"
  - criteria: "Big Idea tem promessa clara"
    description: "Transformação desejável"

# ===== CAMPOS DE RESPOSTA =====
fields:
  - field_id: "product"
    type: "text"
    label: "Nome do produto"
    required: true

  - field_id: "target_audience"
    type: "text"
    label: "Público-alvo"
    required: true

  - field_id: "big_idea"
    type: "textarea"
    label: "Sua Big Idea"
    min_characters: 100
    required: true

  - field_id: "explanation"
    type: "textarea"
    label: "Explicação (por que sua Big Idea é eficaz?)"
    min_characters: 150
    required: true

  - field_id: "reference_link"
    type: "url"
    label: "Link de referência (opcional)"
    required: false

# ===== RECOMPENSAS =====
rewards:
  coins_base: 15
  coins_perfect_score: 30
```

---

## ARQUIVO: srs/vocab.yaml

**Descrição**: Vocabulário para Review SRS (spaced repetition)

### Estrutura

```yaml
# ===== VOCABULÁRIO =====
vocabulary:
  - term: "Big Idea"
    definition: "Ideia central irresistível que conecta produto ao desejo do cliente"
    example: "A dieta que permite comer gordura e perder peso"
    category: "conceito"

  - term: "Novidade"
    definition: "Elemento de uma Big Idea que traz ângulo novo ou surpreendente"
    example: "Ao invés de 'coma menos', a dieta Atkins diz 'coma gordura'"
    category: "framework"

  - term: "Especificidade"
    definition: "Detalhe concreto que torna Big Idea crível e memorável"
    example: "'Perca 5kg em 14 dias' (específico) vs 'Perca peso' (genérico)"
    category: "framework"
```

---

## VALIDAÇÃO AUTOMÁTICA

### Schema JSON (para validar YAML)

Ver arquivo: `/18_GUIA_IMPORTACAO/schema_validacao.json`

### Ferramenta de Validação Online

**Antes de importar**, valide seu YAML:

1. Acesse: https://www.yamllint.com/
2. Cole seu YAML
3. Verifique erros de sintaxe

---

## BOAS PRÁTICAS

### 1. Nomeação de Arquivos

✅ **BOM**:
- `track_001.yaml`
- `lesson_001.yaml`
- `mission_001.yaml`

❌ **RUIM**:
- `Track 1.yaml` (espaço)
- `LESSON-001.YAML` (maiúsculas)
- `missao_001.yaml` (acento)

### 2. IDs Únicos

- Use prefixo consistente: `track_`, `lesson_`, `mission_`, `app_`
- Números com zero à esquerda: `001`, `002`, ..., `010`, `011`

### 3. Texto de Narração

- Use quebra de linha (`|`) para textos longos
- Máximo 300 caracteres por beat (ideal para TTS)
- Evite jargões sem explicação

### 4. Checkpoints

- Todo beat DEVE ter checkpoint (validação obrigatória)
- Feedback deve ser construtivo, não punitivo
- Mínimo 3 opções em QUIZ (idealmente 4)

---

## EXEMPLOS COMPLETOS

Ver pasta: `/18_GUIA_IMPORTACAO/exemplos_completos/`

- `exemplo_basico.yaml`: InfoApp simples (1 track, 3 lessons)
- `exemplo_avancado.yaml`: InfoApp completo (3 tracks, 12 lessons, missions, SRS)

---

## PRÓXIMOS PASSOS

1. **Criar seu primeiro YAML**: Use template de exemplo
2. **Validar**: Rodar validação automática
3. **Importar**: Upload via InfoApp Admin Panel
4. **Testar**: Visualizar no Learner App

---

**Criado por**: EdTech + Data Architect + Tech Writer
**Revisado por**: [Aguardando feedback de criadores]
