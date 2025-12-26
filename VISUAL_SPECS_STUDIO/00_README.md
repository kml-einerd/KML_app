# VISUAL_SPECS_STUDIO

**Documentação Visual-First: Conselho-para-Especificação**
Especificações completas e prontas para desenvolvimento que permitem projetar cada tela no Figma e derivar detalhes de backend/implementação dessas visuais.

---

## PROPÓSITO

Este pacote de documentação permite:

1. **Designers** criar telas Figma completas apenas a partir das especificações textuais
2. **Times de produto** manter consistência com decisões do conselho
3. **Desenvolvedores** entender requisitos de dados/integrações DEPOIS do design visual
4. **Times de QA** validar contra critérios objetivos

---

## PRINCÍPIOS INEGOCIÁVEIS

Todas as especificações refletem estes princípios fundamentais:

1. **Três Apps**: Learner / Creator Studio / Platform Admin (nunca misturar telas/componentes)
2. **Beats + Checkpoints**: Aulas Interativas estruturadas em beats; cada beat tem checkpoint obrigatório
3. **Governança/Quality Gates**: Critérios objetivos bloqueiam publicação (ex: checkpoints faltando)
4. **Sistema de Som**: Documentado por estado (SAFE/TENSION/STATUS) e por ação, com acessibilidade
5. **Modo Studio Edições**: Suporta MicroSaaS (low-ticket) vs produto Full via feature flags/modularidade

---

## ESTRUTURA

```
VISUAL_SPECS_STUDIO/
├── 00_README.md                               (este arquivo)
├── 01_CONSOLIDACAO_CONSELHO.md                (conselho → critérios → implicações de produto)
├── 02_MAPA_NAVEGACAO_OFICIAL.md               (navegação para os 3 apps)
├── 03_SPECS_TELAS/
│   ├── learner/                               (todas as telas do app aluno)
│   ├── creator_studio/                        (todas as telas do studio criador)
│   └── platform_admin/                        (todas as telas admin plataforma)
├── 04_INVENTARIO_COMPONENTES/
│   ├── catalogo_componentes.md                (todos os componentes: props, variantes, estados)
│   ├── placeholders_tokens.md                 (tokens de design: tipografia, espaçamento, cores, etc.)
│   └── estados_e_variantes.md                 (padrões globais: loading, empty, error, success)
├── 05_VARIANTES_MODO_STUDIO/
│   ├── matriz_edicoes.md                      (matriz de funcionalidades: MicroSaaS vs Standard vs Full)
│   ├── regras_modularidade.md                 (como modularizar sem quebrar jornadas)
│   └── templates_micro_saas.md                (3 templates prontos de "produto pequeno")
├── 06_SISTEMA_SOM/
│   ├── mapa_som_por_estado.md                 (cues de som SAFE/TENSION/STATUS)
│   ├── mapa_som_por_acao.md                   (baseados em ação: click, success, checkpoint, etc.)
│   └── regras_acessibilidade.md               (mute, legendas, haptics, preferências)
├── 07_ANALYTICS_LIGHT/
│   └── taxonomia_eventos_por_tela.md          (nomes de eventos + triggers + propriedades por tela)
├── 08_QA_GOVERNANCA/
│   ├── gates_publicacao.md                    (gates objetivos que bloqueiam publicação)
│   └── definicao_pronto_global.md             (DoD para telas, componentes, som, edições)
├── 09_MANIFESTO/
│   └── manifesto_specs_telas.json             (manifesto legível por máquina para automação)
└── 10_QUESTOES_ABERTAS/
    └── riscos_e_incognitas.md                 (incógnitas inevitáveis que requerem decisões futuras)
```

---

## REGRA VISUAL-FIRST (REGRA MESTRA)

Tudo começa pelas visuais de frontend:

- **Para cada tela**: layout/elementos/componentes/estados/CTA primário/som são especificados primeiro
- **Seção "Mapeamento Back Após Visual"** fornece orientação sobre inputs/outputs de dados/ações + integrações sugeridas + atalhos (bibliotecas/templates/plugins)
- Isto é ORIENTAÇÃO, não implementação — detalhes de backend/implementação são derivados DEPOIS do design visual

---

## PADRÃO DE QUALIDADE

Cada entregável é considerado "pronto" quando:

- **Telas**: "Desenhável no Figma" apenas pela spec (layout + elementos + estados + CTA + cues de som)
- **Modo Studio**: Edições são explícitas (o que é removido/simplificado, feature flags, jornadas ainda funcionam)
- **Sistema de Som**: Implementável por estado + por ação, inclui acessibilidade
- **Gates de Publicação**: Objetivos e incluem comportamento de bloqueio claro
- **Rastreabilidade**: Toda decisão principal cita arquivo fonte e tópico

---

## COMO USAR ESTA DOCUMENTAÇÃO

### Para Designers
1. Comece com `02_MAPA_NAVEGACAO_OFICIAL.md` para entender jornadas do usuário
2. Leia specs individuais de telas em `03_SPECS_TELAS/` para cada tela
3. Referencie `04_INVENTARIO_COMPONENTES/` para componentes reutilizáveis
4. Confira `05_VARIANTES_MODO_STUDIO/` para mudanças específicas por edição
5. Revise `06_SISTEMA_SOM/` para cues de áudio/haptics

### Para Gerentes de Produto
1. Revise `01_CONSOLIDACAO_CONSELHO.md` para rastreabilidade de decisões
2. Confira `08_QA_GOVERNANCA/` para quality gates e DoD
3. Use `09_MANIFESTO/manifesto_specs_telas.json` para visão geral
4. Monitore `10_QUESTOES_ABERTAS/` para decisões não resolvidas

### Para Desenvolvedores
1. Comece com design visual (telas Figma)
2. Consulte seção "Mapeamento Back Após Visual" em cada spec de tela
3. Referencie `07_ANALYTICS_LIGHT/` para taxonomia de eventos
4. Confira `04_INVENTARIO_COMPONENTES/estados_e_variantes.md` para padrões
5. Revise `08_QA_GOVERNANCA/gates_publicacao.md` para lógica de validação

---

## RASTREABILIDADE

Todas as specs citam notas do conselho usando este formato:

```
[fonte: NOME_ARQUIVO.md → seção/tópico/paráfrase]
```

Isto garante:
- Nenhuma funcionalidade inventada
- Todas as decisões rastreiam de volta ao consenso do conselho
- Conflitos são detectados cedo
- Visão de produto consistente

---

## TRÊS APPS

### Learner App (Aluno)
Experiência voltada ao estudante: missões, aulas interativas, revisão, progresso, badges.

**Telas principais**: Home (Missão do Dia), Player (Aula Interativa), Checkpoint, Conclusão, Review (SRS), Mapa da Trilha, Progresso, Badges.

### Creator Studio
Ferramentas voltadas ao criador: build, import, preview, QA, publish, analytics.

**Telas principais**: Dashboard, Criar InfoApp, Import Pack, Build (por formato), Preview Emulator, QA Checklist, Publish, Versioning, Analytics, Rewards, Users, Settings, Billing.

### Platform Admin
Ferramentas do operador da plataforma: moderação, marketplace, tenants, infraestrutura.

**Telas principais**: Tenants/Workspaces, Moderação/Trust, Catálogo/Marketplace, Infra/Status, Pagamentos, Auditoria.

---

## CONTATO & FEEDBACK

Para questões ou esclarecimentos sobre estas specs, consulte:
- Matriz de consolidação do conselho (`01_CONSOLIDACAO_CONSELHO.md`)
- Questões abertas (`10_QUESTOES_ABERTAS/riscos_e_incognitas.md`)

---

**Última Atualização**: 2025-12-26
**Versão**: 1.0 (Conselho-para-Spec Final)
