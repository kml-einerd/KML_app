# PLACEHOLDERS & TOKENS DE DESIGN

**Tokens de design: tipografia, espaçamento, cores, etc. - valores a serem populados pelo designer**

Este arquivo define os tokens (variáveis de design) que garantem consistência visual em todo o produto.

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Design Systems Lead → Tokens definidos antecipadamente]

---

## PRINCÍPIO DOS TOKENS

Tokens são variáveis de design reutilizáveis. Ao invés de usar valores diretos (ex: `#FF5733`), usamos tokens (ex: `color.primary`).

**Benefícios**:
1. Consistência garantida
2. Dark mode facilitado (swap de tokens)
3. Variantes por Tension Profile (SAFE/TENSION/STATUS)

[fonte: 07 - alinhamento.md → Head Product Design → Tokens por tension_profile]

---

## 1. CORES

### Cores Primárias (a definir pelo designer)

```
color.primary.500 = [PLACEHOLDER] // Cor principal do produto
color.primary.400 = [PLACEHOLDER] // Variação mais clara
color.primary.600 = [PLACEHOLDER] // Variação mais escura
```

### Cores Neutras (Grayscale)

```
color.neutral.white = #FFFFFF
color.neutral.gray.100 = [PLACEHOLDER] // Background
color.neutral.gray.200 = [PLACEHOLDER]
color.neutral.gray.300 = [PLACEHOLDER]
color.neutral.gray.400 = [PLACEHOLDER] // Borders
color.neutral.gray.500 = [PLACEHOLDER] // Text secondary
color.neutral.gray.600 = [PLACEHOLDER]
color.neutral.gray.700 = [PLACEHOLDER]
color.neutral.gray.800 = [PLACEHOLDER] // Text primary
color.neutral.gray.900 = [PLACEHOLDER]
color.neutral.black = #000000
```

### Cores Semânticas

```
color.success.500 = [PLACEHOLDER] // Verde (checkpoints corretos, conclusões)
color.success.100 = [PLACEHOLDER] // Background suave

color.error.500 = [PLACEHOLDER] // Vermelho (erros, bloqueios)
color.error.100 = [PLACEHOLDER] // Background suave

color.warning.500 = [PLACEHOLDER] // Laranja (alertas, streak em risco)
color.warning.100 = [PLACEHOLDER] // Background suave

color.info.500 = [PLACEHOLDER] // Azul (informações neutras)
color.info.100 = [PLACEHOLDER] // Background suave
```

### Cores por Tension Profile (SAFE/TENSION/STATUS)

[fonte: 01 - economia.md → Cientista Comportamental → Tension Profile]

#### SAFE (reduzir ansiedade, controle)
```
color.tension.safe.primary = [PLACEHOLDER] // Tom calmo (azul/verde suave)
color.tension.safe.background = [PLACEHOLDER] // Background relaxante
color.tension.safe.accent = [PLACEHOLDER] // Acentos sutis
```

#### TENSION (foco, risco simbólico leve)
```
color.tension.medium.primary = [PLACEHOLDER] // Tom ativador (laranja/amarelo)
color.tension.medium.background = [PLACEHOLDER] // Background neutro
color.tension.medium.accent = [PLACEHOLDER] // Acentos moderados
```

#### STATUS (validação, ego)
```
color.tension.status.primary = [PLACEHOLDER] // Tom celebratório (dourado/roxo)
color.tension.status.background = [PLACEHOLDER] // Background rico
color.tension.status.accent = [PLACEHOLDER] // Acentos destacados
```

**Uso**: Player muda tokens visuais conforme estação (SAFE/TENSION/STATUS).

---

## 2. TIPOGRAFIA

### Font Families

```
font.family.sans = [PLACEHOLDER] // Ex: "Inter", "SF Pro", "Roboto"
font.family.mono = [PLACEHOLDER] // Ex: "Fira Code" (se usar código)
```

### Font Sizes (Scale)

```
font.size.xs = 12px
font.size.sm = 14px
font.size.base = 16px // Body text padrão
font.size.lg = 18px
font.size.xl = 20px
font.size.2xl = 24px
font.size.3xl = 30px
font.size.4xl = 36px // Títulos grandes
font.size.5xl = 48px // Headlines
```

### Font Weights

```
font.weight.regular = 400
font.weight.medium = 500
font.weight.semibold = 600
font.weight.bold = 700
```

### Line Heights

```
line.height.tight = 1.2 // Títulos
line.height.normal = 1.5 // Body text
line.height.relaxed = 1.75 // Leitura longa
```

---

## 3. ESPAÇAMENTO (Spacing Scale)

Sistema de espaçamento baseado em múltiplos de 4px (ou 8px).

```
spacing.0 = 0px
spacing.1 = 4px
spacing.2 = 8px
spacing.3 = 12px
spacing.4 = 16px // Padrão interno de cards
spacing.5 = 20px
spacing.6 = 24px
spacing.7 = 28px
spacing.8 = 32px // Padrão entre seções
spacing.10 = 40px
spacing.12 = 48px
spacing.16 = 64px
spacing.20 = 80px
spacing.24 = 96px
```

**Uso**:
- `spacing.4` (16px): Padding interno de cards/botões
- `spacing.8` (32px): Espaçamento entre seções
- `spacing.16` (64px): Espaçamento entre blocos grandes

---

## 4. BORDER RADIUS

```
radius.none = 0px
radius.sm = 4px
radius.base = 8px // Padrão (botões, inputs)
radius.md = 12px
radius.lg = 16px // Cards
radius.xl = 24px
radius.full = 9999px // Círculos (avatars)
```

---

## 5. SOMBRAS (Shadows)

```
shadow.none = none
shadow.sm = [PLACEHOLDER] // Elevation suave
shadow.base = [PLACEHOLDER] // Cards padrão
shadow.md = [PLACEHOLDER] // Modals
shadow.lg = [PLACEHOLDER] // Overlays principais
shadow.xl = [PLACEHOLDER] // Elevation máxima
```

**Exemplo**:
```
shadow.base = 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24)
```

---

## 6. OPACIDADE

```
opacity.0 = 0
opacity.10 = 0.1
opacity.25 = 0.25
opacity.50 = 0.5
opacity.75 = 0.75
opacity.100 = 1
```

**Uso**:
- Overlays de modal: `opacity.50`
- Disabled states: `opacity.50`

---

## 7. Z-INDEX (Camadas)

```
z.base = 0
z.dropdown = 10
z.sticky = 20
z.overlay = 30
z.modal = 40
z.toast = 50
z.tooltip = 60
```

**Uso**: Garante ordem correta de sobreposição.

---

## 8. BREAKPOINTS (Responsividade)

```
breakpoint.mobile = 375px // Mobile small
breakpoint.tablet = 768px // Tablet
breakpoint.desktop = 1024px // Desktop
breakpoint.wide = 1440px // Wide desktop
```

**Media Queries**:
```
@media (min-width: breakpoint.tablet) { ... }
```

---

## 9. TRANSITIONS (Animações)

### Duração

```
transition.duration.fast = 150ms // Micro-feedback (hover)
transition.duration.base = 300ms // Padrão (modals, dropdowns)
transition.duration.slow = 500ms // Animações complexas
```

### Easing (Curvas)

```
transition.ease.linear = linear
transition.ease.in = ease-in
transition.ease.out = ease-out // Mais usado (natural)
transition.ease.inout = ease-in-out
```

**Uso**:
```
transition: all transition.duration.base transition.ease.out;
```

---

## 10. CONTRAST RATIO (WCAG AA)

[fonte: 07 - alinhamento.md → Accessibility → Contraste mínimo (WCAG)]

**Obrigatório**:
- Texto normal (< 18px): Razão mínima 4.5:1
- Texto grande (≥ 18px): Razão mínima 3:1
- Elementos interativos: Razão mínima 3:1

**Validação**: Usar ferramenta como [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

---

## 11. DARK MODE (Opcional mas recomendado)

Tokens permitem implementar dark mode facilmente via swap de valores.

### Light Mode
```
color.background.primary = color.neutral.white
color.text.primary = color.neutral.gray.900
```

### Dark Mode
```
color.background.primary = color.neutral.gray.900
color.text.primary = color.neutral.white
```

**Implementação**: CSS Variables ou tema do framework (Tailwind, styled-components, etc.)

---

## 12. MOTION (Preferências de Acessibilidade)

[fonte: 06_SISTEMA_SOM/regras_acessibilidade.md → Reduce motion]

### Media Query
```
@media (prefers-reduced-motion: reduce) {
  * {
    transition: none !important;
    animation: none !important;
  }
}
```

**Uso**: Respeita preferência do sistema operacional do usuário.

---

## RESUMO: COMO USAR TOKENS

### No Figma
1. Criar **Variables** ou **Styles** para cada token
2. Nomear exatamente como especificado (ex: `color/primary/500`)
3. Usar variáveis em todos os designs

### No Código
1. Usar CSS Variables:
```css
:root {
  --color-primary-500: #FF5733;
  --spacing-4: 16px;
}

.button-primary {
  background: var(--color-primary-500);
  padding: var(--spacing-4);
}
```

2. Ou framework de Design System (Tailwind, Chakra UI, etc.)

---

## DECISÕES PENDENTES (Para Designer Popular)

- [ ] Definir paleta de cores primárias
- [ ] Definir paleta de cores neutras (grayscale)
- [ ] Definir cores por Tension Profile (SAFE/TENSION/STATUS)
- [ ] Escolher font family (sans-serif)
- [ ] Definir sombras (shadow.base, shadow.md, etc.)
- [ ] Validar contrast ratios (WCAG AA)

---

**Última Atualização**: 2025-12-26
