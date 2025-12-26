# SETTINGS (CREATOR STUDIO)

## 1) Objetivo: Configurar domínio/SEO/Store, integrações (TTS), notificações, branding.
[fonte: 03 - todas as telas, flows e governança fechados.md → Configurações]

## 2) Usuário: Creator, **Contexto**: Configurações do InfoApp

## 3) Layout
```
**Domínio & SEO**
Domínio customizado: [_____]
Meta description: [_____]

**Integrações**
TTS: ElevenLabs (conectado)
[Desconectar] [Configurar]

**Notificações Push**
☐ Ativar push notifications

**Branding**
Logo: [Upload]
Cor primária: [#FF5733]

[Salvar]
```

## 4) CTA Primária: "Salvar"
## 5) Estados: Success
## 6) Som: **SAFE**: `save_success.mp3`
## 7) Eventos: `settings_changed`
## 8) DoD: [ ] Domínio customizado, [ ] TTS integration config, [ ] Branding funcional
## 9) Edições: Standard/Full mantém domínio customizado
## 10) Back: `PUT /api/creator/infoapp/:id/settings`

[fonte: 03 - todas as telas, flows e governança fechados.md → Settings]
---
