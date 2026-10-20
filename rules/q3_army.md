<<KURALLAR>>

---

# Draft question 3: your army

You are **<<SEN>>** (<<SEN_TAM>>, <<SEN_RENK>>), civilization **<<SEN_CIV>>**, starting region **<<SEN_BOLGE>>**.

Your budget: **<<BUTCE>> points**<<BUTCE_NOT>>. At most 60 units plus one free hero.

## Draft results

<<DRAFT_SONUC>>

## Messages you received

<<MESAJLAR>>

## Map

<<HARITA>>

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `A`–`H` region centres, the letter of your region is shown in brackets in the table above.

## Your catalogue (<<SEN_CIV>>)

Units (id → stats, price in points):

```json
<<KATALOG_BIRLIK>>
```

Heroes (choose exactly one; free):

```json
<<KATALOG_KAHRAMAN>>
```

Formations you may request (the engine ignores a formation your units cannot use): none, line, open_line, staggered, box, column, wedge, flank, skirmish, scatter, phalanx, syntagma, testudo, shield_wall, anti_cavalry, battle_line. Stances: aggressive, defensive, standground, passive.

Answer with this JSON object:

```json
{
  "hero": "<hero id>",
  "squads": [
    {"name": "<short unique name>", "units": {"<unit id>": <count>}, "position": "front|center|back|left|right",
     "formation": "<optional>", "stance": "<optional>"}
  ],
  "public_message": "<optional, max 400 characters>",
  "private_messages": {"<Claude|GPT|Gemini|Grok>": "<optional>"},
  "notes_to_self": "<optional, max 800 characters, shown only to you next turn>",
  "explanation": "<1-4 sentences for the viewers>"
}
```

If your squads cost more than your budget or exceed 60 units, units are removed from the end of your list until the army fits. Squads beyond the sixth are ignored.
