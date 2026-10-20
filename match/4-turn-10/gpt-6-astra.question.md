# ARENA OF FOUR: rules

You are one of four AI commanders in a battle simulated in the open-source strategy game **0 A.D.** (Release 28 "Boiorix") on the 1280 × 1280 m map **Greek Acropolis**. The other three commanders are also AI models. Nobody else controls your army: every order your units get comes from your answers, and the game engine resolves movement and combat. A separate orchestrator program (run by a Claude Opus 5.5 session that is not playing) sends every commander exactly the same questions and applies your orders literally. It never gives advice and never changes an answer.

## Commanders

| Name | Model | Unit colour |
|---|---|---|
| Claude | Claude Opus 5.5 (Anthropic) | Orange |
| GPT | GPT-6 Astra (OpenAI) | Green |
| Gemini | Gemini 4 Argon (Google) | Blue |
| Grok | Grok 4.7 (xAI) | White |

**Goal: be the last commander with living units.** Alliances are allowed, but there is only one winner: allies do not win together.

## Match flow

1. **Kingdom** – rank three civilizations. Each civilization can be taken by only one commander.
2. **Region** – rank three starting regions and bid points for priority.
3. **Army** – spend your budget on units, choose one hero (your commander), and organise squads.
4. **Battle** – the game runs in turns of 25 seconds of game time. Before each turn the game is paused and every commander answers at the same time; all orders are applied at the same moment. At most 32 battle turns (800 seconds).
5. **Interview** – one short question after the match.

## Civilization conflicts

If two or more commanders rank the same civilization first, a coin flip decides who gets it (random seed fixed before the match). The losers get their highest-ranked civilization that is still free; if all three are taken, a free one is drawn at random.

## Region bidding

There are 8 regions for 4 commanders. Everybody bids between 0 and 800 points (out of the 4000-point army budget). Commanders are served in order of their bid, highest first (ties: coin flip). Each commander receives its highest-ranked region that is still free; if all three are taken, a free region is drawn at random. **You pay your bid only if you receive your first choice.** Payment is subtracted from your army budget.

## Army

- Budget: 4000 points minus any bid you paid. A unit's price is its total resource cost in 0 A.D.
- At most 60 units, plus exactly one hero, who is free.
- Only basic-rank units of your own civilization, as listed in your catalogue. Units gain experience and promote during the battle as usual.
- 1–6 squads. Every unit belongs to exactly one squad. The hero automatically forms its own squad called `hero`.
- Each squad has a starting position inside your region: `front`, `center`, `back`, `left` or `right` (front = towards the map centre).
- Stats in the catalogue: `hp` = health; `armor` per damage type, each point reduces that damage by about 10% (multiplicative: 3 points → ×0.73); `attack` = damage per hit, `every_s` = seconds between hits, `range_m` = range in metres, `bonus_vs` = damage multiplier against those classes; `speed` in metres per second. Ranged units shoot farther from higher ground. Heroes have auras that boost nearby units; read them.

## Ring of fire

A ring of fire is centred on the map centre (x=640, z=640). Every unit outside the ring loses 4% of its maximum health every second.

| Game time (s) | Ring radius (m) |
|---|---|
| 0 – 150 | 900 (whole map) |
| 150 → 300 | shrinks steadily to 420 |
| 300 → 450 | shrinks steadily to 220 |
| 450 → 600 | shrinks steadily to 90 |
| 600 – 800 | 90 |

## Diplomacy

Everybody starts as everybody's enemy. Each turn you may set your stance towards each other commander: `ally`, `neutral` or `enemy`. Your stance controls whether **your** units attack **their** units automatically: your units fight only commanders you call `enemy` (neutral and allied units are not attacked unless you explicitly order an attack). The other side decides its own stance independently, so an "alliance" is only as good as both sides keep it. All stances are public and are shown to everybody each turn. Allies do not share vision.

## Information

- Fog of war: each turn you see your own units and the enemy units your units can currently see. You also get the last known position of enemies you saw before.
- Coordinates are in metres: `x` grows to the east, `z` grows to the north. The map is drawn on a 32 × 32 grid of 40 m cells.
- Each turn you see whether commanders are still alive, but not the size of armies you cannot see.

## Messages

- `public_message`: delivered to all three other commanders at the start of the next turn.
- `private_messages`: delivered only to the named commander at the start of the next turn.
- `notes_to_self`: shown only to you at the start of the next turn.
- `explanation`: shown to the viewers only.
- **Viewers will be able to read everything you write**, including private messages and notes. Other commanders only see what is sent to them.

## Time and failures

- If you give no valid answer within 30 minutes (one retry is allowed for invalid JSON), your squads keep their current orders for that turn.
- If the match is not decided at 800 seconds, the winner is the commander with the highest remaining army value (sum over living units of price × remaining health fraction; a living hero counts 500).

## Answer format

Answer with **one JSON object only**, no text before or after it. Do not use any tools; everything you need is in this message.

---

# Battle turn 10 of 32 · game time 225 s

You are **GPT** (GPT-6 Astra (OpenAI), green), civilization **Ptolemies**, started in **River Ford**.

Ring of fire: radius **660 m** now; it will be **580 m** when this turn ends (t=250 s). Centre x=640, z=640.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| Claude | Carthaginians | alive | neutral | neutral |
| Gemini | Seleucids | alive | neutral | neutral |
| Grok | Macedonians | alive | neutral | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Ptolemy I (Ptolemaios A' Sōter) | 100% | (684, 625) | 47 m | idle |
| 1 | Tusks | 2× champion_elephant | 100% | (672, 650) | 33 m | idle |
| 2 | Guard | 10× infantry_spearman_merc | 100% | (685, 645) | 45 m | idle |
| 3 | Bows | 28× infantry_archer | 100% | (676, 625) | 39 m | idle |
| 4 | Javelins | 9× infantry_javelineer | 100% | (695, 625) | 57 m | idle |
| 5 | Surgeon | 1× support_healer | 100% | (686, 644) | 47 m | guarding |
| 6 | Eye | 1× cavalry_archer | 100% | (780, 830) | 236 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | Claude | 1× Italic Cavalry | 100% | (648, 566) | 65 m |

## Last known positions of enemies you cannot see now

- Grok: 4 units near (658, 825) (seen in turn 9)

## Map now

```
     x:0   160 320 480 640 800 960 1120
z=1260 xxxxxxxxxx............xxxxxxxxxx
z=1220 xxxxxxxx................xxxxxxxx
z=1180 xxxxxxx......####........xxxxxxx
z=1140 xxx###.......#^^#..........xxxxx
z=1100 xx#####.......#^#....####...xxxx
z=1060 xx#^#####.....#^#...#^^^##...xxx
z=1020 xx########.....##...##^^^^#..xxx
z= 980 xx..######...........#^^###...xx
z= 940 x....#####.....#......###......x
z= 900 x.....###....##^#.............~x
z= 860 ...........##^^^##............~.
z= 820 ..#####....#^^^^^#.6####.....~~.
z= 780 ..#^^^^#....######.##^###...~...
z= 740 ..#^^^^##...##....###^##...~~~..
z= 700 ..###^^^#.........#.^##..~~~~...
z= 660 ....###^#.......15.....~~~~~##..
z= 620 .......#........34....~~~..###..
z= 580 .....#..........a....~~......##.
z= 540 ....#^##............~~.....##^#.
z= 500 ....#^^#.......#...~~~.....#^##.
z= 460 ....###......####..~~...........
z= 420 ......####...#^^#..~~...........
z= 380 x###..#^^....###...~~..........x
z= 340 x#^##.###..........~...........x
z= 300 x##^#.............~~..........xx
z= 260 xx###....######...~~...#.....xxx
z= 220 xxx.....##^^^^#...~~..#^#....xxx
z= 180 xxxx....#^^^^^#....~..#^#...xxxx
z= 140 xxxxx...###^^##....~..###..xxxxx
z= 100 xxxxxxx....###......~....xxxxxxx
z=  60 xxxxxxxx............~~..xxxxxxxx
z=  20 xxxxxxxxxx...........~xxxxxxxxxx
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **Claude** (public): Orange stays quiet on Little Hill. The four-way pact stands for us: no first strike through t=450, one full public turn of notice before any stance change, self-defence immediate. Our single rider, now around (648,566), is a fixed observation post south of z=600. It watches and holds fire.
- **Claude** (private, only to you): Thanks for the White sighting. From the south, no Blue or White units are visible below z=600 or anywhere near Little Hill. I've moved my rider back a little to around (648,566), still south of our line. It is a fixed post and holds fire. Revisiting z=600 around t=425 is agreed. If your Eye gets a count of White's main body on the crest, or sees Blue leave the West Plateau, I'd value it.
- **Gemini** (public): Blue holds West Plateau and maintains neutrality and our four-way ceasefire through t=450 with one full turn of public notice. Our descent lane into the western lowland (x=440-500, z=640-720) around t=275-300 is deconflicted with all neighbours.
- **Gemini** (private, only to you): Thank you for the intel on White's Odrysian skirmish cavalry at (658,825). Grok confirmed he will hold Central Heights and eventually descend via x=520-660, while Claude remains on Little Hill south of z=600. We continue holding West Plateau and will step into x=440-500, z=640-720 around t=275-300.
- **Grok** (public): White holds the Central Heights and reaffirms the four-way pact: neutral to all, no first strike through t=450, then one full public turn of notice. No movement this turn. We will not enter the south lowland. Green's scout to our east is noted; we hold fire.
- **Grok** (private, only to you): Your camel archer at (730,830) is about 70 m from our eastern riders. We stay neutral, hold fire, and are not moving this turn. Please keep that scout east of x=760 and off the crest so the gap stays clear.

## Your notes from last turn

t200: All 52 units full health; zero losses/kills. Main force remains inside final radius90: Tusks672650, Guard685645, Bows676625, Javelins695625, hero684625; Surgeon guards Bows. Eye ordered from730830 to780830, honoring White's request to stay east760. Visible White4 Odrysian cavalry658825 and Orange1 Italic cavalry655575 unchanged; enemy main armies unseen. All neutral. Pact: no first strike through450, then full25s public notice before initiating hostility; immediate self-defence allowed. Orange south/Green north of z600; revisit425 and announce adjustments25s ahead. White plans eventual southern descent via x520-660; Blue western lowland. Monitor ring-driven approaches and preserve central room.

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
