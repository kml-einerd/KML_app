# TAXONOMIA DE EVENTOS POR TELA (Analytics LIGHT)

**Eventos Analytics implementados em cada tela**

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Data/Analytics Lead → Taxonomia de eventos desde dia 1]

---

## PRINCÍPIO: ANALYTICS LIGHT

**Light** = eventos essenciais, não telemetria excessiva

**Focos**:
1. Funil de conversão/conclusão
2. Drop-off por beat (crítico)
3. Retenção (D1/D7)

[fonte: 01_CONSOLIDACAO_CONSELHO.md → Data/Analytics Lead → Analytics por beat + drop-off]

---

## EVENTOS GLOBAIS

| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `app_opened` | App abre | `user_id`, `session_id`, `platform` (iOS/Android/Web) |
| `screen_viewed` | Tela carregada | `user_id`, `screen_name`, `timestamp` |

---

## LEARNER APP

### Home
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `home_viewed` | Tela carregada | `user_id`, `streak_dias`, `xp_nivel`, `has_continue_button` |
| `continue_clicked` | Tap "Continuar" | `user_id`, `lesson_id`, `beat_current` |
| `mission_started` | Tap "Começar" MissionCard | `user_id`, `mission_id`, `xp_value` |

### Player
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `player_opened` | Tela carregada | `user_id`, `lesson_id`, `beat_start` |
| `beat_viewed` | Beat exibido | `user_id`, `lesson_id`, `beat_number`, `timestamp` |
| `checkpoint_answered` | Resposta enviada | `user_id`, `lesson_id`, `beat_number`, `checkpoint_type`, `is_correct`, `time_to_answer` |
| `beat_completed` | Avança próximo beat | `user_id`, `lesson_id`, `beat_number`, `duration` |
| `player_exited` | Sai do Player | `user_id`, `lesson_id`, `beat_current`, `exit_reason` (user/error) |

**CRÍTICO**: `checkpoint_answered` e `beat_completed` geram funil de drop-off por beat.

### Conclusão
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `lesson_completed` | Tela carregada | `user_id`, `lesson_id`, `xp_earned`, `bonuses_applied`, `new_level` |

### Review SRS
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `review_item_viewed` | Card exibido | `user_id`, `item_id` |
| `review_item_answered` | Auto-avaliação | `user_id`, `item_id`, `rating` (Acertou/Quase/Errou), `interval_next`, `ease_factor` |

### Aplicação
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `application_started` | Tela carregada | `user_id`, `application_id` |
| `proof_submitted` | Envio de prova | `user_id`, `application_id`, `proof_type` (texto/upload/link) |
| `rubric_evaluated` | Após feedback | `user_id`, `application_id`, `score` |

---

## CREATOR STUDIO

### Dashboard
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `dashboard_viewed` | Tela carregada | `user_id`, `workspace_id`, `apps_count` |
| `app_card_clicked` | Tap em AppCard | `user_id`, `app_id`, `status` (draft/published) |
| `create_new_clicked` | Tap "Criar Novo App" | `user_id`, `workspace_id` |

### Wizard
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `wizard_step_completed` | Avança passo | `user_id`, `step` (1-4), `data` |
| `infoapp_created` | Criação concluída | `user_id`, `app_id`, `niche` |

### Import Pack
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `pack_uploaded` | Upload .zip | `user_id`, `app_id`, `file_size` |
| `pack_validation_started` | Validação iniciada | `user_id`, `app_id` |
| `pack_validation_completed` | Validação concluída | `user_id`, `app_id`, `errors_count`, `warnings_count` |
| `pack_imported` | Import sucesso | `user_id`, `app_id`, `lessons_count` |

### QA Checklist
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `qa_started` | Validação iniciada | `user_id`, `app_id` |
| `qa_completed` | Validação concluída | `user_id`, `app_id`, `gates_passed`, `gates_failed`, `gates_warnings` |

### Publish
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `publish_started` | Inicia publicação | `user_id`, `app_id`, `version` |
| `publish_success` | Publicação sucesso | `user_id`, `app_id`, `version` |
| `marketplace_listed` | Listado marketplace | `user_id`, `app_id` |

### Analytics
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `analytics_viewed` | Tela carregada | `user_id`, `app_id` |
| `tab_changed` | Troca tab | `user_id`, `app_id`, `tab` (funil/drop-off/cohorts) |
| `csv_exported` | Exporta CSV | `user_id`, `app_id` |

---

## PLATFORM ADMIN

### Moderação
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `moderation_queue_viewed` | Tela carregada | `admin_id`, `queue_type` (denuncia/prova) |
| `content_reviewed` | Ação tomada | `admin_id`, `item_id`, `action` (aprovar/reprovar/bloquear) |

### Marketplace
| Evento | Trigger | Propriedades |
|--------|---------|--------------|
| `marketplace_viewed` | Tela carregada | `admin_id` |
| `app_featured` | App marcado featured | `admin_id`, `app_id` |
| `app_approved` | App aprovado | `admin_id`, `app_id` |

---

## RESUMO: EVENTOS CRÍTICOS

**Top 5 para Produto**:
1. `beat_completed` → Drop-off por beat
2. `lesson_completed` → Funil de conclusão
3. `checkpoint_answered` → Validação de aprendizagem
4. `publish_success` → Atividade de criadores
5. `app_opened` → Retenção (D1/D7)

---

**Última Atualização**: 2025-12-26
