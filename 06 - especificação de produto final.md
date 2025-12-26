Fechado. Pra nenhum detalhe ficar de fora, eu vou tratar isso como especificação de produto final (UX/UI \+ Design System \+ Sound System \+ Screen Specs). E sim: Sound Designer entra como membro fixo do conselho porque áudio/haptics vira parte do “motor de estados”.

## **👥 Conselho ampliado (UX/UI \+ áudio)**

* Head Product Design (EdTech/Creator tools)

* UX Architect (IA \+ flows)

* Design Systems Lead (componentes/tokens)

* Motion/Interaction Designer

* UX Writer (microcopy/feedback)

* Growth/Retention Designer (loops)

* Sound Designer (novo) \+ Implementação ElevenLabs/FX (feedback auditivo)

* Accessibility & Inclusive Design

---

# **0\) Regra-mãe do visual**

Não é “app bonito”. É “app claro e inevitável”.

O visual precisa reforçar:

* 1 CTA dominante por tela

* progresso visível

* feedback imediato

* sem poluição / sem gimmick

---

# **1\) Design System final (visual) — tokens e estilo**

## **1.1 Direção de arte (tabela)**

| Item | Diretriz final | Por quê |
| ----- | ----- | ----- |
| Estética | premium, limpo, “tool-like” (não infantil) | escala pra qualquer nicho |
| Densidade | baixa/média, muito respiro | reduz carga cognitiva |
| Hierarquia | título forte \+ 1 CTA | evita indecisão |
| Cards | cantos arredondados, sombra leve | leitura e toque |
| Ilustrações | SVG/anim sutil (não exagerado) | reforça conceito sem distrair |
| Modo | claro \+ escuro desde o início | produto final |

## **1.2 Tipografia e layout (tabela)**

| Token | Padrão |
| ----- | ----- |
| Fonte | system (rápida) ou Inter (profissional) |
| Tamanhos | H1 24–28, H2 18–20, body 14–16 |
| Grid | 8pt grid, spacing consistente |
| Botões | primário sólido, secundário ghost |
| Ícones | set único (line icons), consistentes |

## **1.3 Componentes obrigatórios do Design System (lista curta)**

* MissionCard (CTA do dia)

* ProgressHeader (streak \+ XP do dia)

* TrailMap (etapas \+ bloqueios)

* PlayerFrame (áudio \+ visual \+ beat progress)

* CheckpointModule (Escolha/Quiz/Recall/Simulação)

* FeedbackCard (Explain/Coaching/Reveal)

* XPBreakdownModal (sempre no final)

* BadgeToast \+ BadgeGallery

* SRSReviewCard

* ProofUploader / ProofForm

* QA Gate Banner (bloqueia publish)

* Importer (CSV/JSON/YAML \+ validação)

* Preview Emulator (simula aluno)

* AppStatusPill (draft/beta/published)

---

# **2\) Sound System final (áudio \+ haptics) —** 

# **obrigatório**

Sound não é “enfeite”: é feedback de estado (segurança, tensão, conquista).

## **2.1 Biblioteca de sons (tabela)**

| Cue | Quando toca | Estado psicológico | Observação |
| ----- | ----- | ----- | ----- |
| tap\_soft | clique/ação normal | controle | curtinho, discreto |
| checkpoint\_open | abre checkpoint | foco | “atenção agora” |
| correct\_light | acerto comum | reforço | não infantil |
| wrong\_soft | erro com SAFE | segurança | sem punição emocional |
| wrong\_tension | erro com TENSION | alerta | mais seco, não agressivo |
| streak\_saved | mantém streak | alívio | “ufa” |
| streak\_lost | perde streak | perda | triste, mas leve |
| level\_up | subiu de nível | orgulho | assinatura do produto |
| badge\_unlock | novo badge | identidade | curto \+ marcante |
| activity\_complete | concluiu tarefa | fechamento | prepara XP modal |
| confetti\_pop | confetti | vitória | opcional (toggle) |
| upload\_success | prova enviada | progresso real | “feito” |
| publish\_success | app publicado | conquista do criador | assinatura forte |

## **2.2 Regras de implementação (tabela)**

| Regra | Padrão |
| ----- | ----- |
| Volume | baixo por padrão \+ controle no perfil |
| Mute | toggle global \+ por contexto |
| Haptics | leve (tap), médio (correct), forte (level\_up) |
| Acessibilidade | “reduce motion” \+ “reduce sound cues” |
| Coerência | sons variam por tension\_profile (SAFE/TENSION/STATUS) |
| ElevenLabs | usado para narrativa; FX via biblioteca local/CDN |

## **2.3 Papel do Sound Designer (brief de contratação)**

Ele vai entregar:

* Sound Kit v1 (biblioteca \+ naming \+ guidelines)

* Mix padrão (volumes relativos por cue)

* Mapeamento por estado (SAFE vs TENSION vs STATUS)

* Assinatura sonora (level\_up / publish\_success)

* Testes em devices (celular/com fone/sem fone)

---

# **3\) Especificação de todas as telas (com “o que tem” \+ “visual” \+ “o que usa”)**

Para ficar completo e não virar texto infinito, eu vou usar um template único e preencher tudo.

### **Template de tela (padrão)**

* Objetivo

* Componentes (Design System)

* Ação primária

* Estados (loading/empty/error/success)

* Sons/Haptics

* Eventos (analytics)

---

## **3.1 Learner App — todas as telas**

### **Entrada & conta**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Login | entrar | inputs, primary button, SSO | “Entrar” | tap\_soft | auth\_start/success/fail |
| Signup | criar conta | inputs, termo, SSO | “Criar conta” | tap\_soft | signup\_success |
| Recuperar senha | acesso | input, CTA | “Enviar link” | tap\_soft | reset\_requested |
| Perfil | controle | settings list | — | tap\_soft | settings\_open |
| Configurações | preferências | toggles (sound/haptics/motion) | “Salvar” | tap\_soft | settings\_save |

### **Onboarding**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Objetivo | intenção | cards choice | “Continuar” | tap\_soft | onboarding\_goal |
| Nível | calibrar | slider/choices | “Continuar” | tap\_soft | onboarding\_level |
| Rotina | hábito | time picker | “Definir rotina” | tap\_soft | onboarding\_routine |
| Tutorial streak | evitar perda | modal/coachmarks | “Entendi” | checkpoint\_open | tutorial\_done |

### **Núcleo diário**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Home (Missão do Dia) | retorno diário | MissionCard, ProgressHeader | “Começar missão” | tap\_soft | mission\_start |
| Mapa/Trilha | caminho | TrailMap, AppStatusPill | “Ir para próxima” | tap\_soft | trail\_open |
| Progresso | onde estou | charts simples, stats | — | tap\_soft | progress\_open |
| Badges/Galeria | identidade | BadgeGallery | — | badge\_unlock (quando novo) | badge\_view |

### **Player (unificado)**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| PlayerFrame (Aula/Story) | aprender ativo | PlayerFrame, Progress (beats) | “Continuar” | tap\_soft | lesson\_open, beat\_view |
| CheckpointModule (overlay) | processamento ativo | CheckpointModule, FeedbackCard | “Responder” | checkpoint\_open | checkpoint\_answer |
| Resumo \+ ação aplicada | transferência | closing summary, applied action | “Marcar como feito” | upload\_success (se prova) | action\_submitted |

### **Revisão e aplicação**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Review (SRS) | retenção | SRSReviewCard stack | “Responder próxima” | correct\_light/wrong\_soft | srs\_answer |
| Aplicação (mundo real) | resultado | checklist \+ ProofForm | “Enviar prova” | upload\_success | proof\_submit |
| Histórico/Biblioteca | retomada | list \+ filters | “Continuar” | tap\_soft | content\_resume |

### **Fechamento**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Conclusão (XP breakdown) | reforçar | XPBreakdownModal, BadgeToast | “Fechar” | activity\_complete \+ (badge\_unlock) | activity\_complete |
| Streak em risco | salvar | modal \+ CTA | “Fazer missão” | streak\_saved | streak\_risk |

### **Social (governado)**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Ligas/Ranking | status saudável | leaderboard (TEAM/PRIVATE) | “Ver liga” | tap\_soft | leaderboard\_view |
| Equipe/Grupo | contexto | group card | “Entrar” | tap\_soft | group\_open |

---

## **3.2 Creator Studio — todas as telas**

### **Workspace & Dashboard**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Login Studio | acesso | auth UI | “Entrar” | tap\_soft | studio\_auth |
| Workspace switch | multi-marca | selector | “Selecionar” | tap\_soft | workspace\_select |
| Dashboard | visão geral | cards \+ KPIs | “Criar InfoApp” | tap\_soft | dashboard\_view |

### **Criar / Build**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Create InfoApp (Wizard) | iniciar | form \+ presets | “Criar” | tap\_soft | infoapp\_create |
| Escolher Formato | reduzir opções | format cards | “Continuar” | tap\_soft | format\_select |
| Import/Build (Pacote .zip) | linha de montagem | Importer \+ validação | “Validar” | tap\_soft | import\_upload |
| Build Aula Interativa | gerar cards | beats preview \+ checkpoints | “Gerar” | tap\_soft | lesson\_generate |
| Build Story | narrativa | preview \+ circling | “Gerar” | tap\_soft | story\_generate |
| Build Review SRS | retenção | CSV validator | “Importar” | tap\_soft | srs\_import |
| Build Aplicação | tarefa real | checklist \+ rubrica | “Salvar” | tap\_soft | apply\_create |

### **Preview / QA / Publish**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Preview Emulator | ver como aluno | emulator \+ persona | “Testar fluxo” | tap\_soft | preview\_run |
| QA Checklist | qualidade | QA Gate Banner \+ checklist | “Corrigir” | wrong\_soft (se falha) | qa\_fail/pass |
| Publish | lançar | version \+ notes | “Publicar” | publish\_success | publish\_done |
| Versioning | evoluir | versions list | “Criar nova versão” | tap\_soft | version\_create |

### **Operação (final)**

| Tela | Objetivo | Componentes | Primário | Sons | Eventos |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Analytics | melhorar | funnels \+ drop beats | — | tap\_soft | analytics\_view |
| Rewards/Economia | retenção | rules \+ badges | “Salvar regras” | tap\_soft | economy\_save |
| Users/Cohorts | suporte | cohorts table | — | tap\_soft | cohort\_view |
| Assets Library | reuso | asset manager | “Upload” | tap\_soft | asset\_upload |
| Settings/Integrations | TTS/branding | keys \+ toggles | “Salvar” | tap\_soft | settings\_save |
| Roles & Audit | controle | roles table | “Convidar” | tap\_soft | role\_update |
| Billing | comercial | plan \+ invoices | “Atualizar plano” | tap\_soft | billing\_update |

---

# **4\) “O que será usado em cada tela” (resumo por componentes)**

Se você quiser uma visão ultra objetiva, aqui vai o “mapa componente → telas”:

| Componente | Telas |
| ----- | ----- |
| MissionCard | Home |
| PlayerFrame | Player Aula/Story |
| CheckpointModule | dentro do Player |
| XPBreakdownModal | Conclusão |
| TrailMap | Trilha |
| SRSReviewCard | Review |
| ProofForm/Uploader | Aplicação |
| QA Gate Banner | QA |
| Importer | Import/Build |
| Preview Emulator | Preview |
| BadgeToast/Gallery | Conclusão/Badges |
| ProgressHeader | Home/Progresso |

---

## **Próximo passo (pra avançar de verdade)**

Escolhe por onde você quer que eu detalhe tela por tela com layout textual (wireframe em bullets \+ estados \+ microcopy \+ sons):

1. Learner primeiro (Home → Player → Checkpoints → Conclusão → Review → Aplicação)

2. Creator Studio primeiro (Create → Import Pack → Preview → QA → Publish → Analytics)

Eu recomendo começar por Learner Home \+ Player, porque isso fixa o “idioma visual” do produto.

