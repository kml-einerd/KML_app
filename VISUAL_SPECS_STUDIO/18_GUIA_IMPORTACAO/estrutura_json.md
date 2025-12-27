# Guia de Importação: Estrutura JSON

**Versão**: 1.0
**Data**: 2025-12-27
**Público**: Criadores de conteúdo que preferem JSON a YAML
**Objetivo**: Estrutura de arquivos JSON para importação em massa

---

## YAML vs JSON

### Quando usar YAML?

✅ **Use YAML se**:
- Você edita manualmente (mais legível)
- Precisa de comentários (#)
- Quer menos chaves/vírgulas

**Exemplo YAML**:
```yaml
title: "Meu InfoApp"
language: "pt-BR"  # Idioma principal
```

---

### Quando usar JSON?

✅ **Use JSON se**:
- Você gera programaticamente (script, API)
- Ferramentas externas exigem JSON
- Quer compatibilidade universal

**Exemplo JSON**:
```json
{
  "title": "Meu InfoApp",
  "language": "pt-BR"
}
```

**Nota**: Internamente, YAML e JSON são equivalentes (YAML é superset de JSON).

---

## CONVERSÃO YAML → JSON

### Ferramenta Online

**YAML to JSON**:
- https://www.json2yaml.com/

### Script Python

```python
import yaml
import json

# Ler YAML
with open('manifest.yaml', 'r') as f:
    data = yaml.safe_load(f)

# Escrever JSON
with open('manifest.json', 'w') as f:
    json.dump(data, f, indent=2, ensure_ascii=False)
```

### Script Node.js

```javascript
const yaml = require('js-yaml');
const fs = require('fs');

// Ler YAML
const data = yaml.load(fs.readFileSync('manifest.yaml', 'utf8'));

// Escrever JSON
fs.writeFileSync('manifest.json', JSON.stringify(data, null, 2));
```

---

## ESTRUTURA DE ARQUIVOS JSON

### Pacote de Importação (.zip)

```
infoapp_pack.zip
├── manifest.json           # Metadados do InfoApp
├── tracks/
│   ├── track_001.json      # Módulo 1
│   ├── track_002.json      # Módulo 2
│   └── track_003.json      # Módulo 3
├── lessons/
│   ├── lesson_001.json     # Aula 1
│   ├── lesson_002.json     # Aula 2
│   └── lesson_003.json     # Aula 3
├── missions/
│   ├── mission_001.json    # Missão diária 1
│   └── mission_002.json    # Missão diária 2
├── applications/
│   ├── app_001.json        # Atividade interativa 1
│   └── app_002.json        # Atividade interativa 2
├── srs/
│   └── vocab.json          # Vocabulário para review SRS
└── assets/
    ├── images/
    │   └── cover.png       # Imagem de capa
    └── audios/
        └── intro.mp3       # Áudios customizados
```

---

## ARQUIVO: manifest.json

**Exemplo completo**:

```json
{
  "manifest_version": "1.0",
  "infoapp_id": "copy_big_idea_001",
  "title": "Copy: Big Idea",
  "subtitle": "Aprenda a escrever grandes ideias que vendem",
  "description": "Descubra como criar Big Ideas irresistíveis para seus produtos, usando frameworks provados do mundo do copywriting.",
  "language": "pt-BR",
  "level": "beginner",
  "category": "marketing",
  "branding": {
    "logo": "assets/images/logo.png",
    "cover": "assets/images/cover.png",
    "primary_color": "#6366f1",
    "secondary_color": "#8b5cf6"
  },
  "learning_objectives": [
    "Criar Big Ideas originais usando 5 frameworks",
    "Validar Big Ideas com teste de clareza",
    "Aplicar Big Ideas em campanhas reais"
  ],
  "tracks": [
    "track_001",
    "track_002",
    "track_003"
  ],
  "gamification": {
    "daily_goal_default": 50,
    "streak_bonus_enabled": true,
    "leagues_enabled": true
  },
  "store": {
    "enabled": true,
    "products": [
      {
        "id": "prod_001",
        "name": "Ebook: 100 Big Ideas",
        "price_coins": 100,
        "type": "digital",
        "image": "assets/images/ebook.png"
      }
    ]
  }
}
```

---

## ARQUIVO: tracks/track_001.json

**Exemplo completo**:

```json
{
  "track_id": "track_001",
  "order": 1,
  "title": "Fundamentos da Big Idea",
  "subtitle": "O que é e por que importa",
  "description": "Neste módulo você vai entender o conceito de Big Idea e por que ela é essencial para qualquer campanha de sucesso.",
  "icon": "🚀",
  "cover_image": "assets/images/track_001_cover.png",
  "lessons": [
    "lesson_001",
    "lesson_002",
    "lesson_003"
  ],
  "application": {
    "enabled": true,
    "application_id": "app_001"
  },
  "prerequisites": {
    "track_id": null
  },
  "unlock_conditions": {
    "type": "sequential",
    "required_track": null
  },
  "rewards": {
    "coins": 50,
    "badge": "badge_big_idea_basics"
  }
}
```

---

## ARQUIVO: lessons/lesson_001.json

**Exemplo completo**:

```json
{
  "lesson_id": "lesson_001",
  "track_id": "track_001",
  "order": 1,
  "title": "O que é Big Idea?",
  "subtitle": "Definição e importância",
  "estimated_duration_minutes": 8,
  "objectives": [
    "Definir Big Idea com suas próprias palavras",
    "Identificar Big Ideas em campanhas famosas"
  ],
  "beats": [
    {
      "beat_number": 1,
      "narration": "Big Idea é a ideia central, irresistível, que conecta seu produto ao desejo do cliente.",
      "checkpoint": {
        "type": "QUIZ",
        "prompt": "Qual destas é uma Big Idea?",
        "options": [
          {
            "text": "Produto X é o melhor do mercado",
            "feedback": "Isso é uma afirmação genérica, não uma Big Idea."
          },
          {
            "text": "Descubra o segredo que CEOs usam para dobrar produtividade",
            "feedback": "Correto! Esta é uma Big Idea: específica, curiosa, promete transformação."
          },
          {
            "text": "Compre agora com 50% off",
            "feedback": "Isso é uma oferta, não uma Big Idea."
          }
        ],
        "correct_index": 1
      },
      "audio_url": null,
      "visual_cue": "highlight"
    },
    {
      "beat_number": 2,
      "narration": "Exemplo clássico: 'A dieta que permite comer gordura e perder peso'. Esta Big Idea quebrou o padrão de 'comer menos' e criou um novo mercado.",
      "checkpoint": {
        "type": "REFLECTION",
        "prompt": "Pense em uma Big Idea de um produto que você admira. O que torna ela poderosa?",
        "min_characters": 50
      },
      "audio_url": null,
      "visual_cue": "example"
    }
  ],
  "rewards": {
    "coins_base": 10,
    "coins_perfect_score": 20
  }
}
```

---

## ARQUIVO: applications/app_001.json

**Exemplo completo**:

```json
{
  "application_id": "app_001",
  "track_id": "track_001",
  "title": "Crie sua primeira Big Idea",
  "description": "Aplique o framework de Big Ideas para criar uma ideia original para um produto à sua escolha.",
  "type": "PROJECT",
  "instructions": "1. Escolha um produto (pode ser fictício)\n2. Defina o público-alvo\n3. Crie uma Big Idea usando os 3 elementos (Novidade, Especificidade, Promessa)\n4. Escreva um parágrafo explicando sua Big Idea",
  "checklist": [
    {
      "criteria": "Big Idea tem elemento de novidade",
      "description": "Algo único ou ângulo novo"
    },
    {
      "criteria": "Big Idea é específica (não genérica)",
      "description": "Detalhe concreto, não vago"
    },
    {
      "criteria": "Big Idea tem promessa clara",
      "description": "Transformação desejável"
    }
  ],
  "fields": [
    {
      "field_id": "product",
      "type": "text",
      "label": "Nome do produto",
      "required": true
    },
    {
      "field_id": "target_audience",
      "type": "text",
      "label": "Público-alvo",
      "required": true
    },
    {
      "field_id": "big_idea",
      "type": "textarea",
      "label": "Sua Big Idea",
      "min_characters": 100,
      "required": true
    },
    {
      "field_id": "explanation",
      "type": "textarea",
      "label": "Explicação (por que sua Big Idea é eficaz?)",
      "min_characters": 150,
      "required": true
    },
    {
      "field_id": "reference_link",
      "type": "url",
      "label": "Link de referência (opcional)",
      "required": false
    }
  ],
  "rewards": {
    "coins_base": 15,
    "coins_perfect_score": 30
  }
}
```

---

## VALIDAÇÃO JSON

### Ferramenta Online

**JSON Validator**:
- https://jsonlint.com/

### Script Python

```python
import json

# Validar JSON
try:
    with open('manifest.json', 'r') as f:
        data = json.load(f)
    print("✅ JSON válido!")
except json.JSONDecodeError as e:
    print(f"❌ JSON inválido: {e}")
```

### Script Node.js

```javascript
const fs = require('fs');

// Validar JSON
try {
  const data = JSON.parse(fs.readFileSync('manifest.json', 'utf8'));
  console.log('✅ JSON válido!');
} catch (e) {
  console.error(`❌ JSON inválido: ${e.message}`);
}
```

---

## BOAS PRÁTICAS JSON

### 1. Indentação

✅ **BOM** (2 espaços):
```json
{
  "title": "Meu InfoApp",
  "language": "pt-BR"
}
```

❌ **RUIM** (sem indentação):
```json
{"title":"Meu InfoApp","language":"pt-BR"}
```

---

### 2. Vírgula Final

❌ **RUIM** (vírgula no último item):
```json
{
  "title": "Meu InfoApp",
  "language": "pt-BR",
}
```

✅ **BOM** (sem vírgula no último):
```json
{
  "title": "Meu InfoApp",
  "language": "pt-BR"
}
```

---

### 3. Aspas Duplas

❌ **RUIM** (aspas simples):
```json
{
  'title': 'Meu InfoApp'
}
```

✅ **BOM** (aspas duplas):
```json
{
  "title": "Meu InfoApp"
}
```

---

### 4. Valores Booleanos

❌ **RUIM** (string):
```json
{
  "enabled": "true"
}
```

✅ **BOM** (boolean):
```json
{
  "enabled": true
}
```

---

### 5. Valores Numéricos

❌ **RUIM** (string):
```json
{
  "order": "1"
}
```

✅ **BOM** (number):
```json
{
  "order": 1
}
```

---

## PRÓXIMOS PASSOS

1. **Escolha formato**: YAML (mais legível) ou JSON (mais universal)
2. **Crie arquivos**: Use exemplos acima como template
3. **Valide**: https://jsonlint.com/ ou script Python/Node.js
4. **Importe**: Upload via InfoApp Admin Panel

---

**Criado por**: Data Architect + Tech Writer
**Revisado por**: [Aguardando feedback de criadores]
