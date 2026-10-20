<<KURALLAR>>

---

# Draft question 2: starting region

You are **<<SEN>>** (<<SEN_TAM>>, <<SEN_RENK>>).

## Civilizations

<<CIV_SONUC>>

## Messages you received

<<MESAJLAR>>

## Map

<<HARITA>>

Legend: `.` lowland, `^` high ground (more than 10 m above the plain), `#` cliff or steep slope, `~` river (crossing points are not marked), `A`–`H` region centres (see table). The ring of fire is centred at x=640, z=640.

## Regions

<<BOLGE_TABLOSU>>

Answer with this JSON object:

```json
{
  "regions": ["<id of your first choice>", "<second>", "<third>"],
  "bid": <integer 0-800>,
  "public_message": "<optional, max 400 characters>",
  "private_messages": {"<Claude|GPT|Gemini|Grok>": "<optional, max 400 characters>"},
  "explanation": "<1-4 sentences for the viewers>"
}
```
