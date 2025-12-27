# CONSOLIDAÇÃO DO CONSELHO

**Matriz de Rastreabilidade: Membro do Conselho → Visão → Critérios → Implicações no Produto → Onde Aparece**

Este documento garante que todas as decisões do conselho sejam consistentes, rastreáveis e implementadas em telas, componentes, gates de governança, sistema de som e edições do Studio.

---

## ESTRUTURA COMPLETA DO CONSELHO

[fonte: 08 - conselho e convidados visões.md → Conselho fixo (completo)]

### Membros Fixos do Conselho (14)

| Área | Função | Responsabilidade |
|------|--------|------------------|
| Produto/Visão | Head of Product | Visão geral do produto, governança, consistência |
| UX/IA | Arquiteto UX | Navegação, arquitetura de informação, fluxos |
| UI/Design System | Design Systems Lead | Inventário de componentes, tokens, consistência visual |
| Interação/Motion | Motion/Interaction Designer | Microinterações, transições, ritmo dos beats |
| Conteúdo/UX Writing | UX Writer / Content Designer | Microcopy, feedback, tom, clareza |
| Retenção/Economia | Growth & Retention Designer + Game Economy | Loops, notificações, XP, badges, streaks |
| Psicologia/Aprendizagem | Cientista de Aprendizagem | Processamento ativo, checkpoints, transferência |
| Ferramentas do Criador | Especialista em Creator Studio | Creator Loop, import, templates, modularidade |
| Engenharia/Arquitetura | Staff Engineer / Arquiteto | Viabilidade técnica, atalhos, integrações |
| Dados/Analytics | Data/Tracking Lead | Taxonomia de eventos, funis, analytics |
| Trust/Safety | Trust & Safety Lead | Moderação, anti-abuso, reportes |
| Áudio | Sound Designer | Sistema de som por estado e ação |
| Acessibilidade | Accessibility Lead | WCAG, reduce motion/sound, inclusividade |
| Operações | PMO/Program Manager | Marcos, DoD, cadência de entrega |

### Convidados Fixos (3)

| Área | Função | Por Que Fixo |
|------|--------|--------------|
| Editorial | Head of Content Ops / Editorial Lead | Qualidade escala via padrões editoriais + revisão por amostra |
| Legal/Compliance | Legal/Compliance Lead | UGC, marketplace, privacidade (LGPD), consentimento, políticas de conteúdo |
| Gestão de Custos | FinOps/Infra Cost Lead | Governança de custos ElevenLabs + assets + storage em escala |

### Convidado Sob Demanda (1)

| Área | Função | Quando Necessário |
|------|--------|-------------------|
| Tooling | Developer Experience (DX) / Tooling | Implementação do Import Pack (validação fácil, mensagens de erro claras) |

---

## MATRIZ DE CONSOLIDAÇÃO

### 1) Head of Product

[fonte: 09 - visão final e condições.md → Head of Product (Produto)]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Motor (Tema→Estação→Dinâmica→Tarefa) é estável | Arquitetura do produto não pode ser "builder genérico" | 3 Formatos Oficiais: Missão, Aula Interativa, Aplicação | 01 - economia.md → Constituição do Sistema; Creator Studio: Create InfoApp, telas Build; Learner: telas Player, Missão, Aplicação |
| Quality Gates bloqueiam publicação | Checkpoint por beat obrigatório; Aplicação por módulo obrigatória; Feedback obrigatório | QA Checklist previne apps ruins em escala | Creator Studio: tela QA Checklist, tela Publish (estado bloqueado); 08_QA_GOVERNANCA/gates_publicacao.md |
| 80+ apps (escala conteúdo-agnóstico) | Creator Loop via Import Pack (.zip) | Criação baseada em templates, não construção livre de app | Creator Studio: tela Import Pack; 05_VARIANTES_MODO_STUDIO/templates_micro_saas.md |
| Loja de Recompensas [fonte: Resposta Cliente #3] | Aluno troca XP/moedas por produtos/benefícios (NÃO é catálogo de apps) | Loja de recompensas + configuração de produtos | Learner: tela Marketplace/Loja; Creator Studio: Rewards/Economy (ativar loja); Platform Admin: Configuração de Produtos |

**Risco se ignorado**: Produto se torna "builder genérico" sem diferenciação.

---

### 2) Arquiteto UX

[fonte: 09 - visão final e condições.md → UX Architect (IA/Flows)]
[fonte: 07 - alinhamento.md → UX Architect]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| 3 apps nunca se misturam | Learner vs Creator Studio vs Platform Admin devem ser separados | Navegação, branding, componentes não podem se misturar | 02_MAPA_NAVEGACAO_OFICIAL.md; todas as telas separadas por app em 03_SPECS_TELAS/ |
| Player unificado com modos | Checkpoint é componente, não tela separada | PlayerFrame único gerencia Aula Interativa, Story, com CheckpointModule overlay | Learner: tela Player; 04_INVENTARIO_COMPONENTES/catalogo_componentes.md (PlayerFrame, CheckpointModule) |
| Navegação Oficial (tabs + sidebar) | Learner: bottom tabs; Studio: sidebar; "Continuar" sempre visível | Padrões de navegação consistentes previnem "app labirinto" | 02_MAPA_NAVEGACAO_OFICIAL.md; Learner: tela Home (botão Continuar); Creator: Dashboard (sidebar) |
| Search global | Produto final precisa de descoberta rápida de conteúdo | Search + filtros nos apps Learner e Creator | Learner: tela Search; Creator Studio: Dashboard (barra de busca) |
| 1 CTA dominante por tela | Evita paralisia de decisão | Cada tela tem exatamente uma ação primária | Todas as specs de tela: seção "5) Ação Primária" |

**Risco se ignorado**: Excesso de caminhos cria confusão e abandono.

---

### 3) Head Product Design (UX/UI)

[fonte: 09 - visão final e condições.md → Head Product Design]
[fonte: 07 - alinhamento.md → Head Product Design]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Premium, multi-nicho, tool-like (não infantil) | Estética escala para qualquer nicho (negócios, saúde, criativo, etc.) | Direção de design limpa, baixa densidade, profissional | 06 - especificação.md → Design System → Direção de arte; 04_INVENTARIO_COMPONENTES/placeholders_tokens.md |
| Tokens por tension_profile (SAFE/TENSION/STATUS) | Clima visual muda sem redesign | Variantes de token para cada estado de tensão | 04_INVENTARIO_COMPONENTES/placeholders_tokens.md (variantes de tensão); 06_SISTEMA_SOM/ (som por estado) |
| 1 CTA dominante por tela | Hierarquia clara, sem competição visual | Hierarquia visual forte com botão dominante único | Todas as specs de tela: seção de layout |

**Risco se ignorado**: Identidade visual inconsistente entre apps Learner/Studio/Admin.

---

### 4) Design Systems Lead

[fonte: 09 - visão final e condições.md → Design Systems Lead]
[fonte: 07 - alinhamento.md → Design Systems Lead]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Componentes travados antes de polimento de pixels | Design System com componentes obrigatórios previne deriva | Catálogo finito de componentes com props/variantes/estados | 04_INVENTARIO_COMPONENTES/catalogo_componentes.md |
| Tokens (spacing/type/color) + dark mode | Tokens definidos antecipadamente para consistência e tematização | Placeholders de tokens para designer popular | 04_INVENTARIO_COMPONENTES/placeholders_tokens.md |
| Estados padrão (empty/error/loading/success) | Padrões de estado padrão com componentes prontos | Tratamento global de estados documentado | 04_INVENTARIO_COMPONENTES/estados_e_variantes.md |
| Variantes SAFE/TENSION/STATUS | Variantes de token por perfil de tensão | Componentes se adaptam visualmente ao estado psicológico | 04_INVENTARIO_COMPONENTES/placeholders_tokens.md; todas as specs de tela: "8) Som/Haptics" |

**Risco se ignorado**: Cada tela inventa novos padrões → explosão de manutenção.

---

### 5) Motion/Interaction Designer

[fonte: 09 - visão final e condições.md → Motion/Interaction Designer]
[fonte: 07 - alinhamento.md → Motion/Interaction Designer]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Beats + checkpoints como ritmo do produto | Ritmo de interação espelha ritmo de aprendizagem (beats de 45-90s) | Transições de beat (snap + progresso) criam sensação de avanço | Learner: tela Player; 04_INVENTARIO_COMPONENTES/catalogo_componentes.md (PlayerFrame, progresso de beat) |
| Motion sutil, condicional, respeita "reduce motion" | Design de motion accessibility-first | Motion com media query prefers-reduced-motion; confetti apenas em marcos | 06_SISTEMA_SOM/regras_acessibilidade.md; todas as telas: "8) Som/Haptics" |
| Confetti condicional (não sempre) | Previne infantilização e fadiga | Confetti apenas em level_up/desbloqueio de badge, não toda conclusão | Learner: tela Conclusão; 06_SISTEMA_SOM/mapa_som_por_acao.md |
| Micro-feedback (shake, glow) no checkpoint | Reforço sem texto extra | Cues visuais em respostas corretas/erradas | Learner: componente CheckpointModule; 04_INVENTARIO_COMPONENTES/catalogo_componentes.md |

**Risco se ignorado**: Animação excessiva se torna distração ou infantilização.

---

### 6) UX Writer / Content Designer

[fonte: 09 - visão final e condições.md → UX Writer]
[fonte: 07 - alinhamento.md → UX Writer]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Feedback "o que isso revela" (identidade > certo/errado) | Feedback foca no que a resposta revela sobre o aluno | Copy de feedback enfatiza crescimento e insight, não apenas correção | Learner: CheckpointModule, FeedbackCard; todas as specs de tela: "7) Conteúdo / Microcopy" |
| Remover tom infantil (parabéns!!!) | Produto premium multi-nicho precisa de tom profissional | Microcopy é claro, encorajador, profissional (não infantil) | Todas as specs de tela: "7) Conteúdo / Microcopy"; 04_INVENTARIO_COMPONENTES/estados_e_variantes.md |
| Texto curto + claro por beat | Áudio narra; texto guia | Texto breve otimizado para narração em áudio | Learner: beats da tela Player; Import Pack: estrutura lessons/*.yaml |
| Microcopy padrão para erros, checkpoints, QA gates, publish | Consistência em todo o produto | Biblioteca de microcopy para cenários comuns | 04_INVENTARIO_COMPONENTES/estados_e_variantes.md; 08_QA_GOVERNANCA/gates_publicacao.md |

**Risco se ignorado**: Texto genérico faz experiência parecer "curso fatiado" não aprendizagem ativa.

---

### 7) Cientista de Aprendizagem

[fonte: 09 - visão final e condições.md → Learning Scientist]
[fonte: 05 - sistema completo de EdTech.md → Quality Gates]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Checkpoint = processamento ativo (não consumo passivo) | Todo beat deve ter checkpoint; aula sem checkpoint = publicação bloqueada | Checkpoint por beat é regra de validação obrigatória | Creator Studio: tela QA Checklist; 08_QA_GOVERNANCA/gates_publicacao.md (Gate 1: Atividade) |
| Aplicação com prova (transferência pro mundo real) | Cada módulo precisa de pelo menos 1 Aplicação com prova/rubrica | Formato Aplicação é Formato Oficial; prova obrigatória | Learner: tela Aplicação; Creator Studio: tela Build Aplicação |
| Objetivo de aprendizagem claro por trilha | Toda trilha define "o que você saberá fazer no final" | Metadados da trilha incluem declaração de objetivo | Creator Studio: wizard Create InfoApp; Platform Admin: Catálogo (exibição de objetivo) |
| Beats: 8 beats de 45-90s | Ritmo previne consumo passivo de "vídeo longo" | Validação de timing do beat no import | 01 - economia.md → tabela Neuro/Aprendizagem; validação Import Pack; 08_QA_GOVERNANCA/gates_publicacao.md (Gate 5: Tempo/escopo) |

**Risco se ignorado**: Produto perde diferenciação científica; vira consumo passivo de conteúdo.

---

### 8) Growth & Retention Designer

[fonte: 09 - visão final e condições.md → Growth/Retention Designer]
[fonte: 07 - alinhamento.md → Growth/Retention Designer]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Streak/badges/recompensas com governança | Economia serve o hábito, não o grind; sem ranking público | Ranking apenas TEAM/PRIVATE; sistema de streak com notificação "em risco" | Learner: Home (ProgressHeader com streak), tela Progresso, tela Badges; 08_QA_GOVERNANCA/gates_publicacao.md (validação de economia) |
| Ranking público removido | Risco de vergonha/toxicidade eliminado | Tela Ligas/Ranking mostra apenas ligas TEAM/PRIVATE | Learner: tela Ligas/Ranking; 04_INVENTARIO_COMPONENTES/catalogo_componentes.md (componente leaderboard) |
| Calendário/agenda semanal (planejamento) | Sensação de controle e compromisso | Tela de planejamento semanal | Learner: tela Agenda/Calendário (nova) |
| Notificações contextuais (missão, streak em risco, unlock) | Retenção via relevância, não spam | Sistema de notificação por tipo de gatilho | Learner: Settings (preferências de notificação); todas as telas: "9) Eventos" (triggers) |

**Risco se ignorado**: Economia vira grind/cassino; competição tóxica gera churn.

---

### 9) Game Economy / LiveOps

[fonte: 09 - visão final e condições.md → Game Economy / LiveOps]
[fonte: 01 - economia.md → Game Designer, Economista de jogos/LiveOps]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| XP base por arquétipo | Missão: 10-20; Aula Interativa: 50-80; Aplicação: 120-200 | Regras de XP são simples e previsíveis | Creator Studio: tela Rewards/Economy; 01 - economia.md → tabela Game Designer |
| Bônus simples (streak, primeira ação, conclusão perfeita) | Evitar bônus agressivos de velocidade que criam ansiedade | Multiplicadores: streak 1.05x-1.2x; bônus primeira ação (fixo); bônus conclusão perfeita (fixo) | 01 - economia.md → tabela Multiplicadores; Learner: tela Conclusão (breakdown de XP) |
| Sem bônus agressivos de velocidade/comparação | Incentivo errado cria comportamento errado (pressa, fraude, churn) | Bônus de velocidade opcional e leve; sem comparação pública | 08_QA_GOVERNANCA/gates_publicacao.md (regras de economia) |

**Risco se ignorado**: Estrutura de incentivos gera engajamento apressado e superficial e potencial fraude.

---

### 10) Sound Designer

[fonte: 09 - visão final e condições.md → Sound Designer]
[fonte: 06 - especificação.md → Sistema de Som final]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Sons como parte do motor de estado | Som é feedback de estado (segurança, tensão, conquista) não decoração | Cues de som por tension_profile (SAFE/TENSION/STATUS) e por ação | 06_SISTEMA_SOM/mapa_som_por_estado.md, mapa_som_por_acao.md |
| Sound Kit v1 + mix + toggle no player | Biblioteca de som completa com mix de volume; controle do usuário in-context | Biblioteca de som com convenções de nomenclatura; player tem toggle de som | 06_SISTEMA_SOM/mapa_som_por_acao.md; Learner: tela Player (toggle de som); Settings (mute global) |
| Sem sons de erro punitivos/agressivos | Som punitivo mata motivação de aprendizagem | Sons de erro são coaching (suaves, não ásperos) | 06_SISTEMA_SOM/mapa_som_por_acao.md (wrong_soft, wrong_tension); todas as telas: "8) Som/Haptics" |
| Acessibilidade obrigatória | Mute, reduzir cues de som, alternativas hápticas | Controles de acessibilidade para som/haptics | 06_SISTEMA_SOM/regras_acessibilidade.md; Learner: Settings (toggles de som/haptics/motion) |

**Risco se ignorado**: Áudio punitivo prejudica aprendizagem; inacessibilidade exclui usuários.

---

### 11) Accessibility Lead

[fonte: 09 - visão final e condições.md → Accessibility Lead]
[fonte: 07 - alinhamento.md → Accessibility]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Controles para som/haptics/motion | Acessibilidade real, não conformidade de checkbox | Toggles de Settings para reduce-motion, reduce-sound, mute, haptics | Learner: tela Settings; 06_SISTEMA_SOM/regras_acessibilidade.md |
| Legendas no player | Inclusividade + ambientes silenciosos | Player mostra legendas/subtítulos para narração em áudio | Learner: tela Player (toggle de legendas); 04_INVENTARIO_COMPONENTES/catalogo_componentes.md (PlayerFrame) |
| Contraste mínimo (WCAG) | Legibilidade para todos os usuários | Razões de contraste de token atendem mínimo WCAG AA | 04_INVENTARIO_COMPONENTES/placeholders_tokens.md (tokens de cor) |
| "Consertar agora, não depois" | Dívida de acessibilidade é impossível de consertar retroativamente | Acessibilidade construída em cada componente e tela desde o início | 08_QA_GOVERNANCA/definicao_pronto_global.md (checklist de acessibilidade) |

**Risco se ignorado**: Dívida de acessibilidade se torna impossível de consertar; exclui base de usuários significativa.

---

### 12) Trust & Safety Lead

[fonte: 09 - visão final e condições.md → Trust & Safety / Moderation]
[fonte: 07 - alinhamento.md → Trust & Safety / Anti-abuse]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Moderação de conteúdo | Conteúdo gerado por usuário exige moderação para proteger reputação | Reportes, revisão de conteúdo suspeito, bloqueios | Platform Admin: tela Moderação/Trust; Learner: Reportar Problema (reporte in-app) |
| Anti-fraude em aplicações | Aplicações auto-declarativas podem ser abusadas (texto copiado, links spam) | Detecção de padrões suspeitos (SEM upload de arquivos) | Learner: tela Aplicação (validação de texto/link); Platform Admin: Moderação (conteúdo suspeito) |
| Logs de auditoria no Studio | Times criadores multi-usuário precisam de accountability | Trilha de auditoria para todas as ações do Studio | Creator Studio: tela Roles & Audit; Platform Admin: tela Audit |

**Risco se ignorado**: UGC sem moderação se torna risco reputacional e legal; times criadores carecem de accountability.

[fonte: Resposta Cliente #2 → Upload de prova REMOVIDO, validação via honestidade + padrões suspeitos]

---

### 13) Staff Engineer / Arquiteto

[fonte: 09 - visão final e condições.md → Staff Engineer / Architect]
[fonte: 04 - modo Studio.md → Modelo de importação em massa]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Estrutura Import Pack (.zip) | Criação de conteúdo escalável via pacote estruturado | manifest.yaml + tracks.yaml + lessons/*.yaml + missions/*.yaml + applications/*.yaml + srs/vocab.csv + assets/ | Creator Studio: tela Import Pack; 05_VARIANTES_MODO_STUDIO/templates_micro_saas.md (estrutura do pack) |
| Versioning + Preview Emulator | Gestão de release profissional | Publish cria versões; emulator simula experiência do aluno | Creator Studio: tela Preview Emulator, tela Versioning |
| QA gates validam antes de publish | Automação bloqueia conteúdo ruim | Validação checa estrutura, checkpoints, feedback, timing | Creator Studio: tela QA Checklist; 08_QA_GOVERNANCA/gates_publicacao.md |
| Atalhos via bibliotecas/templates/plugins | Viabilidade via reuso, não build customizado | Atalhos recomendados em "Mapeamento Back Após Visual" para cada tela | Todas as specs de tela: "12) Mapeamento Back Após Visual → atalhos recomendados" |

**Risco se ignorado**: Começar sem contratos = retrabalho caro; experiência do criador é dolorosa.

---

### 14) Data/Analytics Lead

[fonte: 09 - visão final e condições.md → Data/Analytics Lead]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Analytics por beat + drop-off + cohorts | Produto deve provar "edtech de resultado" com dados | Taxonomia de eventos por tela; tracking de funil por beat | 07_ANALYTICS_LIGHT/taxonomia_eventos_por_tela.md; Creator Studio: tela Analytics (drop-off por beat) |
| Taxonomia de eventos + dashboards desde dia 1 | Sem telemetria, não pode provar valor ou otimizar | Cada spec de tela inclui "9) Eventos (Analytics LIGHT)" | Todas as specs de tela: seção "9) Eventos" |
| Dashboards-alvo (target dashboards) | Criadores e plataforma precisam de insights acionáveis | Creator Studio Analytics: funil de conclusão, drop-off por beat, cohorts, heatmaps | Creator Studio: tela Analytics |

**Risco se ignorado**: Não pode provar valor; não pode otimizar retenção; voando cego.

---

### 15) Content Ops / Editorial Lead

[fonte: 09 - visão final e condições.md → Content Ops / Editorial Lead]
[fonte: 05 - sistema completo de EdTech.md → Padrões editoriais]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Padrões editoriais + revisão por amostra | Qualidade escala via padrões, não apenas automação | Guia editorial + rubricas; QA para todos os apps publicados | 08_QA_GOVERNANCA/gates_publicacao.md (QA editorial); 05 - EdTech.md → tabela Padrões editoriais |
| Objetivos de aprendizagem obrigatórios | Toda trilha deve definir objetivo | Metadados da trilha obrigam "o que você será capaz de fazer" | Creator Studio: wizard Create InfoApp (campo objetivo obrigatório); 08_QA_GOVERNANCA/gates_publicacao.md (Gate 6: Clareza) |
| Exemplos reais obrigatórios | Mínimo de 1 exemplo real por aula | Validação checa presença de exemplo | 08_QA_GOVERNANCA/gates_publicacao.md (padrões editoriais) |
| Linguagem clara (clareza > jargão) | Produto multi-nicho requer linguagem acessível | Diretrizes editoriais para criadores | Creator Studio: Content Library (doc de diretrizes editoriais) |

**Risco se ignorado**: Escala sem padrões editoriais = catálogo fraco; perda de confiança.

---

### 16) Legal/Compliance Lead

[fonte: 09 - visão final e condições.md → Legal/Compliance]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Termos para UGC e loja de recompensas | Fundação legal para conteúdo gerado por usuário e loja | Termos de Serviço, políticas de conteúdo, consentimento de dados | Learner: Signup (checkboxes de consentimento); Creator Studio: Publish (termos de uso); Platform Admin: Moderação (políticas de conteúdo) |
| Privacidade (conformidade LGPD) | Conformidade com regulação de proteção de dados | Política de privacidade, controles de dados, gestão de consentimento | Learner: Signup (consentimento de privacidade), Settings (controles de privacidade); Creator Studio: Settings (configurações de dados) |
| Políticas de conteúdo + moderação | Proteger plataforma de risco legal | Enforcement de política de conteúdo via moderação | Platform Admin: tela Moderação/Trust (enforcement de políticas) |

**Risco se ignorado**: UGC + loja sem fundação legal = bomba legal.

[fonte: Resposta Cliente #2 → Upload REMOVIDO; Resposta Cliente #3 → Loja de Recompensas]

---

### 17) FinOps/Infra Cost Lead

[fonte: 09 - visão final e condições.md → FinOps/Infra Cost]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Quotas por plano + políticas de custo | ElevenLabs + assets + storage podem explodir custos em escala | Limites de quota por tier de plano; caching; alertas | Creator Studio: tela Billing (quotas do plano); Settings (uso de quota TTS) |
| Alertas para estouro de custo | Explosão silenciosa de custo é risco de negócio | Monitoramento e alertas de custo | Platform Admin: tela Infra/Status (alertas de custo) |
| Caching + limites | Prevenir chamadas de API redundantes e bloat de storage | Caching de assets; caching de resultado TTS; limites de storage | Todas as telas usando TTS/assets: seção de mapeamento back nota caching |

**Risco se ignorado**: Custo explode silenciosamente com ElevenLabs e assets em escala.

---

### 18) PMO/Program Manager

[fonte: 09 - visão final e condições.md → PMO/Program Manager]
[fonte: 01 - economia.md → PMO/Operações]

| Visão | Critério (Inegociável) | Implicação no Produto | Onde Aparece |
|-------|------------------------|----------------------|--------------|
| Marcos com Definition of Done | Cada entregável tem critérios objetivos de "pronto" | DoD para telas, componentes, som, edições | 08_QA_GOVERNANCA/definicao_pronto_global.md |
| Pacote de documentos antes do build | Alinhar specs antes da implementação para evitar retrabalho e divergência | PRD, IA, Design System, Import Spec, ERD, OpenAPI entregues primeiro | 01 - economia.md → tabela PMO/Operações (cadência Sprint) |
| Entregáveis de Sprint em tabelas | Formato consistente para entregáveis | Entregáveis tabulares por sprint | 01 - economia.md → tabela PMO/Operações |

**Risco se ignorado**: Construir antes do alinhamento = atrasos e divergência.

---

## RESOLUÇÃO DE CONFLITOS

Todas as visões do conselho foram revisadas para conflitos. Nenhum encontrado. Todas as decisões se alinham e reforçam:

1. **Aprendizagem ativa** (checkpoints, aplicação) sobre consumo passivo
2. **Governança** (quality gates) sobre criação livre
3. **Simplicidade** (poucos formatos, economia simples) sobre complexidade
4. **Premium** (profissional, acessível) sobre design infantil ou excludente
5. **Escala** (80+ apps via import) sobre craft manual

---

## RESUMO: O QUE O CONSELHO EXIGE

[fonte: 09 - visão final e condições.md → Mensagem final unificada]

Construa isto como um **motor governado**, não um editor solto.

Os diferenciais são inegociáveis:

1. **Aula Interativa** = beats + checkpoint obrigatório por beat
2. **Aplicação** = prova + rubrica + feedback
3. **Creator Loop** = Import Pack em escala + QA gates bloqueando publicação
4. **Economia simples** + áudio por estado + acessibilidade + trust/moderação

Se estes quatro forem "leis" no código, você tem edtech real e escalável.

---

**Última Atualização**: 2025-12-26
