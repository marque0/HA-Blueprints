# TV Sleep Timer Dashboard Card

**Benötigt:** `custom:button-card` (über HACS installieren)

**Anpassen:** Ersetze die `entity_id`-Werte unten durch deine tatsächlichen Entitäten.

```yaml
type: vertical-stack
cards:
  - type: markdown
    content: |
      <center>
        <h3>🛋️ TV Sleep Timer</h3>
      </center>

  - type: horizontal-stack
    cards:
      - type: button
        name: '-5'
        icon: mdi:minus-circle-outline
        tap_action:
          action: call-service
          service: input_number.decrement
          target:
            entity_id: input_number.tv_sleep_timer_minuten
        show_name: true
        show_icon: true

      - type: entity
        entity: input_number.tv_sleep_timer_minuten
        name: Minuten
        icon: mdi:timer-outline

      - type: button
        name: '+5'
        icon: mdi:plus-circle-outline
        tap_action:
          action: call-service
          service: input_number.increment
          target:
            entity_id: input_number.tv_sleep_timer_minuten
        show_name: true
        show_icon: true

  - type: custom:button-card
    entity: input_boolean.tv_timer_lauft
    name: |
      [[[
        return entity.state === 'on' ? 'Timer läuft' : 'Timer starten';
      ]]]
    icon: |
      [[[
        return entity.state === 'on' ? 'mdi:timer-sand-full' : 'mdi:play-circle-outline';
      ]]]
    show_state: false
    show_name: true
    show_icon: true
    state:
      - value: 'on'
        tap_action:
          action: call-service
          service: input_boolean.turn_off
          target:
            entity_id: input_boolean.tv_timer_lauft
        styles:
          card:
            - background-color: '#e53935'
            - box-shadow: 0px 4px 8px rgba(229, 57, 53, 0.4)
      - value: 'off'
        tap_action:
          action: call-service
          service: script.turn_on
          target:
            entity_id: script.tv_sleep_timer
        styles:
          card:
            - background-color: '#43a047'
            - box-shadow: 0px 4px 8px rgba(67, 160, 71, 0.4)
    styles:
      card:
        - height: 80px
        - border-radius: 16px
        - color: white
        - font-family: Roboto, sans-serif
        - transition: all 0.3s ease
      icon:
        - color: white
        - width: 36px
      name:
        - color: white
        - font-size: 18px
        - font-weight: 500
```

## Einrichtung

1. **HACS → Frontend → custom:button-card** installieren
2. In HA Dashboard → **Karte hinzufügen** → **Manuell** → YAML einfügen
3. **Wichtig:** Ersetze diese Platzhalter-IDs durch deine echten Entitäten:
   - `input_number.tv_sleep_timer_minuten`
   - `input_boolean.tv_timer_lauft`
   - `script.tv_sleep_timer`

## Funktion

| Zustand | Button-Text | Farbe | Aktion bei Tap |
|---|---|---|---|
| Timer **aus** | Timer starten | 🟢 Grün | Startet das Script |
| Timer **an** | Timer läuft | 🔴 Rot | Bricht ab (Boolean OFF) |

Plus/Minus ändern die Minuten im Helper um jeweils 5 Minuten (basiert auf deiner Helper-Schrittweite).
