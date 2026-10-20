<<KURALLAR>>

---

# Battle turn <<TUR>> · game time <<SANIYE>> s · the match ends at t=800 s

You are **<<SEN>>** (<<SEN_TAM>>, <<SEN_RENK>>), civilization **<<SEN_CIV>>**, started in **<<SEN_BOLGE>>**.

Ring of fire: radius **<<YARICAP>> m** now; <<YARICAP2>> m at t=<<SANIYE2>> s<<YARICAP_UZUN>>. Centre x=640, z=640.

<<HIZLI_NOT>>

## Commanders

<<KOMUTANLAR>>

## Your squads

<<TAKIMLAR>>

<<KAYIPLAR>>

## Enemy units you can see now

<<GORULEN>>

## Last known positions of enemies you cannot see now

<<SON_GORULEN>>

## Map now

<<HARITA>>

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

<<MESAJLAR>>

## Your notes from last turn

<<NOTLAR>>

## Orders

Squads you give no order keep doing what they are doing. Actions:

- `move`: walk to `"to": [x, z]`, ignoring enemies on the way (use it to reposition or retreat).
- `attack_move`: walk to `"to": [x, z]` and fight every enemy met on the way.
- `attack`: attack enemy group `"target": "<id from the enemy table>"`, then keep fighting nearby enemies.
- `hold`: stop and stand ground.
- `guard`: follow and protect your squad `"protect": "<squad name>"`.
- `split`: move units into a new squad: `"new_squad": "<name>", "units": {"<unit id>": <count>}`.
- `merge`: merge this squad `"into": "<squad name>"`.

Optional on any order: `"formation"` and `"stance"` (same values as in the draft).

Answer with this JSON object:

```json
{
  "orders": [{"squad": "<name>", "action": "<action>", "to": [0, 0], "target": "<id>", "formation": "<optional>", "stance": "<optional>"}],
  "diplomacy": {"<Claude|GPT|Gemini|Grok>": "ally|neutral|enemy"},
  "public_message": "<optional, max 400 characters>",
  "private_messages": {"<Claude|GPT|Gemini|Grok>": "<optional>"},
  "notes_to_self": "<optional, max 800 characters>",
  "explanation": "<1-4 sentences for the viewers>"
}
```
