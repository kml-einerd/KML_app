Fechado — sem MVP. Vamos tratar como produto final (v1 completo), com todas as telas, flows e governança fechados de forma publicável.

Abaixo eu chamo o conselho ampliado (os perfis mais adequados pra UX/UI) e já faço a revisão crítica do mapa de telas que listamos — ajustando o que precisa pra ficar coerente com:
	•	Motor (Tema→Estação→Dinâmica→Tarefa)
	•	Formatos Oficiais
	•	Aula Interativa em beats + checkpoints
	•	Creator Loop (linha de montagem)
	•	Escala (80+ apps) conteúdo-agnóstico

⸻

👥 Conselho ampliado (UX/UI) — “só gente de produto final”

Núcleo (5):
	1.	Head of Product Design (EdTech/Creator Tools)
	2.	UX Architect (IA + navegação)
	3.	Design Systems Lead (UI escalável + componentes)
	4.	Growth & Retention Designer (loops, notificações, economia saudável)
	5.	UX Writer / Content Designer (microcopy, instruções, feedback)

Convidados especializados (para fechar “final”):
6) Accessibility & Inclusive Design (WCAG, legibilidade, motion-safe)
7) Motion/Interaction Designer (beats, transições, confetti, microfeedback)
8) Research/Insights (Jobs-to-be-Done) (criador e aluno)
9) Anti-abuse/Trust UX (ranking, prova, upload, fraude, moderação)
10) B2B/Admin UX (workspaces, roles, billing, auditoria)

⸻

1) Revisão do mapa de telas — ajustes necessários (produto final)

1.1 Problema do mapa anterior (diagnóstico do Conselho)

Ele estava bom, mas “parecia MVP” porque:
	•	misturava “telas” com “funcionalidades” (ex: checkpoint overlay é componente, não tela)
	•	não fechava Workspaces/Roles, Billing, Versionamento, QA gates, Moderation, Distribuição/Store
	•	não explicitava Navegação oficial (como o usuário circula)

✅ Ajuste: vamos organizar em 3 Apps (Learner / Creator Studio / Platform Admin) + camadas transversais.

⸻

2) Arquitetura final de informação (IA) — visão completa (tabela)

2.1 Learner App (Aluno) — seções finais

Seção	Telas	Observações UX (final)
Entrada	Login, Signup, Recuperar, Consentimentos	login social + SSO opcional
Onboarding	Objetivo, Nível, Rotina, Preferências	“safe by default”; notifs opt-in
Core Diário	Home (Missão do Dia), “Continuar”, “Próximo Passo”	1 CTA dominante
Conteúdo	Player Aula Interativa, Player Story, Review SRS, Aplicação	Player único com “modes”
Progresso	Mapa/Trilha, Progresso, Badges/Galeria, Histórico	“onde estou / o que falta”
Economia	Recompensas/Loja, Carteira/Saldo, Histórico de resgates	regras claras e anti-frustração
Social (governado)	Ligas/Ranking (TEAM/PRIVATE), Equipe/Grupo	sem vergonha pública
Feedback & Suporte	Feedback rápido, Reportar problema, Ajuda/FAQ, Contato	dentro do fluxo, não escondido
Conta	Perfil, Configurações, Privacidade, Notificações	controle total

✅ Mudança importante: “Checkpoint/Quiz overlay” vira componente do Player, não tela solta.

⸻

2.2 Creator Studio (Criador) — seções finais

Seção	Telas	Observações UX (final)
Workspace	Login, Selecionar workspace, Criar workspace	multi-produto, multi-marca
Dashboard	Visão geral, Apps (lista), Atalhos (criar/importar/publish)	métricas de produto
Criar InfoApp	Wizard: nome, nicho, idioma, branding, público	criador não “monta app”
Build (Linha de montagem)	Import/Builder por Formato Oficial	templates e validação fortes
Preview	Preview learner (Home/Player/Trilha)	preview por perfil (SAFE/TENSION/STATUS)
QA & Governança	Checklist, Warnings, Bloqueios, “Corrigir agora”	gates obrigatórios (ex: aula sem checkpoint bloqueia)
Publish & Versões	Publish, Versioning, Changelog, Rollback	release profissional
Conteúdo & Biblioteca	Content Library, Assets, Reuso de blocos	escala (80 apps)
Economia & Recompensas	XP rules, Badges, Loja	presets + avançado
Analytics	Funil por beat, drop-off, cohorts, heatmaps	otimização contínua
Usuários & Comunidade	usuários, cohorts, feedback, suporte	moderação e insights
Configurações	domínio, integrações (TTS), notificações, SEO/Store	distribuição
Billing	plano, limites, faturas	v1 completo exige isso
Roles & Audit	permissões, logs, trilha de auditoria	obrigatório pra escalar time


⸻

2.3 Platform Admin (Operador da plataforma) — seções finais

Seção	Telas	Pra quê
Tenants/Workspaces	gestão global	suporte e compliance
Moderation/Trust	denúncias, bloqueios, revisão de conteúdo	evitar abuso
Catálogo/Marketplace	listagem de infoapps	descoberta
Infra/Status	saúde do sistema	operação
Pagamentos	planos, cobrança, chargebacks	negócio
Auditoria	logs e incidentes	segurança


⸻

3) Navegação oficial (para não virar “app labirinto”)

3.1 Navegação do Aluno (tabela)

Padrão	Tabs principais	Entradas rápidas	Regra
Bottom tabs (5)	Home • Trilha • Review • Progresso • Perfil	“Continuar” sempre visível	1 CTA dominante por tela
Player	modal/stack interno	checkpoint dentro do player	checkpoint bloqueia avanço

3.2 Navegação do Criador (tabela)

Padrão	Menu lateral	Barra superior	Regra
Sidebar (8–10)	Apps • Build • Preview • QA • Publish • Analytics • Users • Rewards • Settings • Billing	workspace switch + search	“Create → Build → QA → Publish” sempre linear


⸻

4) Componentes UI obrigatórios (Design System) — produto final

Componente	Onde usa	Observação
Mission Card	Home	estado: pendente/concluída
Progress Bar (beats)	Player	por beat e total
Checkpoint Module	Player	escolha/quiz/recall/simulação
Feedback Module	Player/Conclusão	EXPLAIN / COACHING / REVEAL
XP Breakdown Modal	final	obrigatório
Streak Widget	Home/Progresso	“em risco” com notificação
Badge Toast + Gallery	app todo	identidade
SRS Card	Review	acerto/erro e intervalo
QA Gate Banner	Studio	“bloqueado até corrigir”
Template Importer	Studio	CSV/JSON + validação
Preview Emulator	Studio	simula telas do aluno


⸻

5) Ajustes recomendados pelo Conselho (o que mudar agora)

5.1 Ajustes de estrutura (tabela)

Item	Antes	Agora (ajuste final)	Por quê
Aula/Story	telas separadas	1 Player com modos	consistência + escala
Checkpoint	“tela”	componente do Player	reduz complexidade
Review	opcional	seção fixa	retenção estrutural
Aplicação	“extra”	formato oficial	resultado real
QA/Publish	“pode depois”	obrigatório	protege a fábrica
Billing/Roles	v2	v1 completo	produto final exige


⸻

6) Próximo passo (concreto) — sem MVP, mas com ordem inteligente

Agora que as telas estão “completas”, o próximo passo é especificar cada tela com:
	•	Componentes
	•	Estados (loading/empty/error/success)
	•	Ação primária
	•	Regras do motor (estação/tension/checkpoint)
	•	Eventos (analytics)

Entregável imediato da próxima rodada

Eu posso te devolver, em tabela, o detalhamento completo das 6 telas núcleo (que definem a experiência final):

Aluno: Home • Player • Conclusão
Criador: Create InfoApp • Build/Import • QA/Publish

E depois a gente expande pras demais com o mesmo template.

Só me diga: você quer começar detalhando Aluno ou Criador primeiro?