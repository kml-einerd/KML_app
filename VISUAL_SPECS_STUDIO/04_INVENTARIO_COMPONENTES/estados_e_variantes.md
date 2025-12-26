# ESTADOS E VARIANTES (Padrões Globais)

**Padrões globais de estados (Loading, Empty, Error, Success) e variantes aplicáveis a todos os componentes**

Este documento define como TODOS os componentes e telas devem lidar com estados, garantindo consistência.

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Design Systems Lead → Estados padrão com componentes prontos]

---

## PRINCÍPIO DE ESTADOS

Todo componente/tela DEVE implementar ao menos 3 estados:
1. **Loading**: Carregando dados
2. **Empty**: Sem dados para exibir
3. **Error**: Falha ao carregar/executar
4. **Success**: Estado padrão (dados carregados)

**Exceções**: Componentes puramente visuais (ex: Button, Icon) não precisam de todos os estados.

---

## 1. LOADING (CARREGANDO)

### Quando usar
- Ao buscar dados do backend
- Ao processar ação (ex: salvar, publicar)
- Ao gerar conteúdo (ex: TTS)

### Padrões visuais

#### Skeleton (Preferido)
Mostra estrutura da tela/componente enquanto carrega.

**Vantagens**:
- Usuário entende o que está carregando
- Reduz ansiedade (vs spinner genérico)

**Componente**: `<Skeleton variant="text|card|circle" />`

**Exemplo**:
```
┌─────────────────────────────┐
│ [████████████] (skeleton)   │ ← Nome (texto)
│ [████] (skeleton)           │ ← Avatar (círculo)
│ [██████████████] (skeleton) │ ← Card
└─────────────────────────────┘
```

#### Spinner (Alternativo)
Ícone rotativo genérico. Usar apenas se:
- Ação é rápida (< 2s)
- Skeleton não faz sentido (ex: botão "Salvando...")

**Componente**: `<Spinner size="small|medium|large" />`

### Microcopy
- Loading state: Sem texto (skeleton fala por si)
- Spinner: "Carregando...", "Salvando...", "Publicando..."

### Duração esperada
- **< 1s**: Ideal (não precisa skeleton, apenas spinner)
- **1-3s**: Skeleton recomendado
- **> 3s**: Skeleton + progresso (se possível)

[fonte: 03 - todas as telas, flows e governança fechados.md → Estados padrão]

---

## 2. EMPTY (SEM DADOS)

### Quando usar
- Lista vazia (ex: "Nenhum InfoApp criado ainda")
- Busca sem resultados
- Filtro sem match

### Padrões visuais

**Estrutura**:
1. Ilustração (opcional mas recomendado)
2. Título claro
3. Descrição curta (1-2 linhas)
4. CTA (se aplicável)

**Exemplo**:
```
┌─────────────────────────────┐
│     [Ilustração]            │
│                             │
│   Nenhum InfoApp criado     │
│   Comece criando seu        │
│   primeiro app.             │
│                             │
│   [Criar Novo App]          │
└─────────────────────────────┘
```

### Microcopy

**Bom**:
- "Nenhum InfoApp criado ainda"
- "Crie seu primeiro InfoApp"
- "Sua busca não encontrou resultados"

**Evitar**:
- "Ops! Parece que está vazio aqui!" (muito casual)
- "Nada aqui..." (frustrante)

### Tom
- Neutro, encorajador
- Sempre oferecer próxima ação

---

## 3. ERROR (ERRO)

### Quando usar
- Falha ao carregar dados
- Falha ao salvar/publicar
- Erro de validação

### Padrões visuais

**Estrutura**:
1. Ícone de erro (⚠️ ou X)
2. Mensagem clara do que deu errado
3. Ação para resolver (ex: "Tentar novamente")

**Exemplo**:
```
┌─────────────────────────────┐
│        ⚠️                    │
│                             │
│ Não conseguimos carregar    │
│ sua missão do dia.          │
│                             │
│ Verifique sua conexão e     │
│ tente novamente.            │
│                             │
│ [Tentar Novamente]          │
└─────────────────────────────┘
```

### Tipos de erro

#### Erro de Rede
**Mensagem**: "Não conseguimos conectar. Verifique sua internet e tente novamente."
**CTA**: "Tentar Novamente"

#### Erro de Validação
**Mensagem**: "Verifique os campos: [lista de campos]"
**CTA**: Nenhum (usuário corrige campos)

#### Erro de Servidor (500)
**Mensagem**: "Algo deu errado. Já fomos notificados e estamos resolvendo."
**CTA**: "Voltar" ou "Tentar Novamente"

#### Erro de Permissão (403)
**Mensagem**: "Você não tem permissão para acessar isso."
**CTA**: "Voltar"

### Microcopy

**Bom**:
- "Não conseguimos carregar [recurso]"
- "Verifique [ação] e tente novamente"
- Clara, acionável

**Evitar**:
- "Erro 500" (jargão técnico)
- "Algo deu errado" (sem contexto)
- Tom punitivo ou dramático

### Tom
- Calmo, empático
- Sempre oferecer caminho para resolver

[fonte: 07 - alinhamento.md → UX Writer → Microcopy padrão para erros]

---

## 4. SUCCESS (SUCESSO / PADRÃO)

### Quando usar
- Estado padrão (dados carregados corretamente)
- Após ação bem-sucedida (salvar, publicar)

### Feedback de Sucesso (Toast/Banner)

**Quando usar**:
- Ação significativa concluída (ex: "Versão publicada")
- Feedback temporário (auto-dismiss)

**Estrutura**:
```
┌─────────────────────────────┐
│ ✓ Versão v1.1 publicada!    │
└─────────────────────────────┘
```

**Duração**: 3-5s (auto-dismiss)
**Tom**: Encorajador mas profissional (não infantil)

[fonte: 07 - alinhamento.md → UX Writer → Remover tom infantil]

---

## 5. DISABLED (DESABILITADO)

### Quando usar
- Botão/campo temporariamente indisponível
- Ação bloqueada por validação

### Padrões visuais
- `opacity: 0.5`
- Cursor: `not-allowed`
- Sem hover/interação

**Acessibilidade**:
- `aria-disabled="true"`
- Tooltip explicando por que está desabilitado

**Exemplo**:
```
[Botão: "Publicar" (disabled)]
Tooltip: "Corrija os erros do QA Checklist antes de publicar"
```

---

## 6. FOCUS (FOCO - Acessibilidade)

### Obrigatório para acessibilidade
Todo elemento interativo (botão, input, link) DEVE ter estado de foco visível.

### Padrões visuais
- Outline: `2px solid color.primary.500`
- Offset: `2px` (não colar no elemento)

**CSS**:
```css
:focus-visible {
  outline: 2px solid var(--color-primary-500);
  outline-offset: 2px;
}
```

**Evitar**:
- `outline: none` (SEM exceções, NUNCA)

[fonte: 07 - alinhamento.md → Accessibility → Acessibilidade obrigatória]

---

## 7. HOVER (DESKTOP)

### Quando usar
- Botões
- Links
- Cards clicáveis

### Padrões visuais
- **Botão Primary**: Background mais escuro (`primary.600`)
- **Botão Secondary**: Border mais escuro
- **Link**: Underline
- **Card**: Sombra maior (`shadow.md`)

**Transição**: `transition.duration.fast` (150ms)

**Mobile**: Não aplicar (touch não tem hover)

---

## 8. PRESSED / ACTIVE (CLICADO)

### Quando usar
- Feedback imediato ao clicar/tocar

### Padrões visuais
- **Botão**: Scale down (`transform: scale(0.98)`)
- **Card**: Sombra menor (`shadow.sm`)

**Duração**: Apenas durante o clique/toque

---

## 9. VARIANTES POR TENSION PROFILE

[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile]

Componentes do Player mudam tokens visuais conforme estação (SAFE/TENSION/STATUS).

### SAFE (Reduzir Ansiedade)
- Cores: Tons calmos (azul/verde suave)
- Feedback: Suave, sem pressa
- Som: `ambient_safe.mp3`

### TENSION (Foco, Risco Leve)
- Cores: Tons ativadores (laranja/amarelo)
- Feedback: Moderado, consequência leve
- Som: `ambient_tension.mp3`

### STATUS (Validação, Ego)
- Cores: Tons celebratórios (dourado/roxo)
- Feedback: Rico, destacado
- Som: `ambient_status.mp3`

**Uso**: PlayerFrame troca `tension_profile` conforme estação da aula.

[fonte: 04_INVENTARIO_COMPONENTES/placeholders_tokens.md → Cores por Tension Profile]

---

## 10. DARK MODE (Opcional)

### Se implementar dark mode
- Trocar tokens de cor via CSS Variables ou tema
- Testar contrast ratio (WCAG AA) em ambos os modos

**Exemplo**:
```css
/* Light mode */
:root {
  --color-background: #FFFFFF;
  --color-text: #000000;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-background: #1A1A1A;
    --color-text: #FFFFFF;
  }
}
```

---

## 11. MICRO-FEEDBACK (ANIMAÇÕES SUTIS)

[fonte: 07 - alinhamento.md → Motion Designer → Micro-feedback (shake, glow)]

### Checkpoint Correto
- **Visual**: Glow verde suave
- **Haptic**: Success (médio)
- **Som**: `correct_soft.mp3` ou `correct_tension.mp3`

### Checkpoint Errado
- **Visual**: Shake leve (não punitivo)
- **Haptic**: Light impact
- **Som**: `wrong_soft.mp3`

**CSS Animation (Shake)**:
```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

.checkpoint-wrong {
  animation: shake 0.3s ease-in-out;
}
```

**Acessibilidade**: Respeitar `prefers-reduced-motion`.

---

## 12. BIBLIOTECA DE MICROCOPY PADRÃO

[fonte: 07 - alinhamento.md → UX Writer → Microcopy padrão]

### Loading
- "Carregando..."
- "Salvando..."
- "Publicando..."

### Empty
- "Nenhum [item] encontrado"
- "Crie seu primeiro [item]"
- "Sua busca não retornou resultados"

### Error
- "Não conseguimos carregar [recurso]"
- "Verifique sua conexão e tente novamente"
- "Algo deu errado. Tente novamente."

### Success
- "[Ação] concluída!"
- "Salvo com sucesso"
- "Versão publicada!"

### Validation
- "Este campo é obrigatório"
- "Email inválido"
- "Senha deve ter ao menos 8 caracteres"

---

## RESUMO: CHECKLIST DE ESTADOS

Para cada componente/tela:

- [ ] **Loading**: Skeleton ou spinner implementado
- [ ] **Empty**: Ilustração + mensagem + CTA
- [ ] **Error**: Mensagem clara + ação para resolver
- [ ] **Success**: Estado padrão funcional
- [ ] **Disabled**: Visual + tooltip explicativo
- [ ] **Focus**: Outline visível (WCAG)
- [ ] **Hover**: Feedback visual (desktop)
- [ ] **Pressed**: Micro-feedback ao clicar

---

**Última Atualização**: 2025-12-26
