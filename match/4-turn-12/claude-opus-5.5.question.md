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

# Battle turn 12 of 32 · game time 275 s

You are **Claude** (Claude Opus 5.5 (Anthropic), orange), civilization **Carthaginians**, started in **Little Hill**.

Ring of fire: radius **500 m** now; it will be **420 m** when this turn ends (t=300 s). Centre x=640, z=640.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| GPT | Ptolemies | alive | neutral | neutral |
| Gemini | Seleucids | alive | neutral | neutral |
| Grok | Macedonians | alive | neutral | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Hannibal Barca (Ḥannibaʿal Baraq) | 100% | (591, 445) | 201 m | idle |
| 1 | swords | 18× infantry_swordsman_gaul, 18× infantry_swordsman_ital, 1× infantry_javelineer_iber | 100% | (598, 434) | 210 m | idle |
| 2 | spears | 20× infantry_spearman_ital | 100% | (601, 453) | 191 m | idle |
| 3 | elephant | 1× champion_elephant | 100% | (609, 429) | 213 m | idle |
| 4 | scout | 1× cavalry_spearman_ital | 100% | (678, 584) | 68 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | GPT | 1× hero Ptolemy I (Ptolemaios A' Sōter), 2× Towered War Elephant, 10× Mercenary Thureos Spearman, 28× Cretan Mercenary Archer, 9× Mercenary Thureos Skirmisher, 1× Surgeon | 100% | (681, 630) | 46 m |

## Last known positions of enemies you cannot see now

(none)

## Map now

```
     x:0   160 320 480 640 800 960 1120
z=1260 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1220 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1180 xxxxxxxxxxxxx####xxxxxxxxxxxxxxx
z=1140 xxx###xxxxxxx#xx#xxxxxxxxxxxxxxx
z=1100 xx#####xxxx...#^#....####xxxxxxx
z=1060 xx#x#####.....#^#...#^^x##xxxxxx
z=1020 xx########.....##...##^^xx#xxxxx
z= 980 xxxx######...........#^^###xxxxx
z= 940 xxxxx#####.....#......###.xxxxxx
z= 900 xxxxx.###....##^#..........xxx~x
z= 860 xxxxx......##^^^##.........xxx~x
z= 820 xx#####....#^^^^^#..####....x~~x
z= 780 xx#x^^^#....######.##^###...~xxx
z= 740 xx#x^^^##...##....###^##...~~~xx
z= 700 xx###^^^#.........#.^##..~~~~xxx
z= 660 xxxx###^#..............~~~~~##xx
z= 620 xxxx...#.........a....~~~..###xx
z= 580 xxxx.#..........4....~~.....x##x
z= 540 xxxx#^##............~~.....##x#x
z= 500 xxxx#^^#.......#...~~~.....#x##x
z= 460 xxxx###......#02#..~~.......xxxx
z= 420 xxxxx.####...#13#..~~......xxxxx
z= 380 x###x.#^^....###...~~......xxxxx
z= 340 x#x##x###..........~......xxxxxx
z= 300 x##x#xx...........~~.....xxxxxxx
z= 260 xx###xxx.######...~~...#xxxxxxxx
z= 220 xxxxxxxx##^^^^#...~~..#x#xxxxxxx
z= 180 xxxxxxxx#xx^^^#....~.x#x#xxxxxxx
z= 140 xxxxxxxx###xx##xxxx~xx###xxxxxxx
z= 100 xxxxxxxxxxx###xxxxxx~xxxxxxxxxxx
z=  60 xxxxxxxxxxxxxxxxxxxx~~xxxxxxxxxx
z=  20 xxxxxxxxxxxxxxxxxxxxx~xxxxxxxxxx
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **GPT** (public): Green holds around x672-695, z625-650; scout holds at (780,830). No advance or obstruction of the announced western lanes. Pact reaffirmed: no first strike through t450, then a full 25-second public notice before hostilities; immediate self-defence reserved.
- **GPT** (private, only to you): Acknowledged, and thanks for the corridor information. Our positions and z600 boundary remain unchanged; revisit around t425.
- **Gemini** (public): Blue: Beginning our announced descent off the West Plateau this turn into the western edge of our agreed lane (x=430-450, z=670-710). Holding well west of x=460 and north of z=660. Pact reaffirmed: neutral to all, no first strike through t=450, and one full turn of public notice before any hostility.
- **Gemini** (private, only to you): Beginning our descent off the plateau this turn to the western edge of our box (x=430-450, z=670-710). No eyes on White yet from the plateau, but our northern screen at (440,710) will be near the western foot of the Heights next turn—will share anything we spot.
- **Grok** (public): White holds the Central Heights. No movement. Neutral to all. Pact unchanged: no first strike through t=450, then one full public turn of notice. I will not enter the west lane (x=440-500) or descend the south corridor (x=520-660) without announcing one turn prior. Every White squad stays east of x=520 and north of z=780.
- **Grok** (private, only to you): No LOS off the crest: I cannot see Blue, your rider, or Green's body. Last Green contact was one unit near (730,830) on turn 9; they claim the scout is now at (780,830), east of x=760 — unverified. Your report of Green at ~(680,620), archer-heavy, is the picture I am using. Please tell me the same turn if they cross north of z=700 or shift into x=520-660. I am not coming south.

## Your notes from last turn

T11 t250 quiet. GPT FULL comp seen (681,630): Ptolemy, 2 towered eleph, 10 thureos sp, 28 Cretan arch, 9 skirm, 1 surgeon (+1 E scout). Sent comp to Gemini+Grok (priv). Pact: no 1st strike to t450, then 1 public turn notice -> earliest attack t475; keep it. z600 line w/ GPT (me S), revisit ~t425. Gemini: W plateau -> x440-500 z640-720 ~t275-300, beside Grok lane x520-660. Grok: crest, descends w/ notice, won't enter my S. RISK: neutral=no auto-fight; surprise volley from GPT 28 arch -> stay >100m from GPT until stances enemy. Plan: T12 rider W ~(510,595) watch Gemini, T14 back E ~(650,580). Leave hill T18 (t425) via N exit (580,510), only choke; out before t460. Ring t300 r420 t425 r253 t450 r220 t475 r198 t550 r133 t600 r90. Never ally.

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
