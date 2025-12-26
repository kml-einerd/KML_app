# REGRAS DE ACESSIBILIDADE (SOM/HAPTICS/MOTION)

**Controles obrigatórios para acessibilidade real**

[fonte: 07 - alinhamento.md → Accessibility → Controles obrigatórios]

---

## 1. MUTE GLOBAL

**Onde**: Settings → Som [ON/OFF]
**Comportamento**:
- OFF: Silencia TODOS os sons (ambiente + cues + feedback)
- ON: Sons tocam conforme volume mix

**Persistência**: Salva preferência (não resetar a cada sessão)

---

## 2. TOGGLE DE SOM NO PLAYER

**Onde**: Player (top bar) → Ícone 🔊/🔇
**Comportamento**:
- Mute temporário (apenas no Player)
- Não afeta som global (Settings)

**Uso**: Ambientes sem som permitido (biblioteca, transporte público)

---

## 3. REDUCE SOUND CUES

**Onde**: Settings → Som → Reduzir Cues [ON/OFF]
**Comportamento**:
- ON: Remove sons ambientes e cues (tap, swipe)
- Mantém: Feedback importantes (correct, wrong, complete)

**Uso**: Sensibilidade auditiva

---

## 4. HAPTICS [ON/OFF]

**Onde**: Settings → Haptics [ON/OFF]
**Comportamento**:
- OFF: Desliga toda vibração
- ON: Haptics conforme specs

**Nota**: Respeita configuração do sistema operacional

---

## 5. REDUCE MOTION

**Onde**: Settings → Motion [Reduzir/Normal]
**Comportamento**:
- Reduzir: Remove animações (confetti, transições)
- Normal: Animações completas

**CSS**:
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

[fonte: 07 - alinhamento.md → Motion Designer → Respeita "reduce motion"]

---

## 6. LEGENDAS NO PLAYER

**Onde**: Player → Toggle Legendas [ON/OFF]
**Comportamento**:
- ON: Exibe legendas sincronizadas com áudio
- OFF: Apenas áudio

**Obrigatório**: Inclusividade + ambientes silenciosos

[fonte: 07 - alinhamento.md → Accessibility → Legendas no player]

---

## RESUMO: TOGGLES OBRIGATÓRIOS

- [ ] Mute Global (Settings)
- [ ] Toggle Som no Player
- [ ] Reduce Sound Cues (Settings)
- [ ] Haptics ON/OFF (Settings)
- [ ] Reduce Motion (Settings)
- [ ] Legendas ON/OFF (Player)

---

**Última Atualização**: 2025-12-26
