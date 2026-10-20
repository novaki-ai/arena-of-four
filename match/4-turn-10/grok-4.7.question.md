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

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **660 m** now; it will be **580 m** when this turn ends (t=250 s). Centre x=640, z=640.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| Claude | Carthaginians | alive | neutral | neutral |
| GPT | Ptolemies | alive | neutral | neutral |
| Gemini | Seleucids | alive | neutral | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Alexander III the Great (Alexandros ho Megas) | 100% | (569, 825) | 198 m | guarding |
| 1 | sarissa | 9× infantry_pikeman | 100% | (568, 812) | 187 m | idle |
| 2 | crete | 11× infantry_archer | 100% | (545, 855) | 235 m | idle |
| 3 | volley | 10× infantry_archer | 100% | (600, 842) | 206 m | idle |
| 4 | thessaly | 4× cavalry_spearman | 100% | (508, 832) | 233 m | idle |
| 5 | odrysia | 4× cavalry_javelineer | 100% | (658, 825) | 186 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

(none)

## Last known positions of enemies you cannot see now

- GPT: 1 units near (730, 830) (seen in turn 9)

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
z= 860 ...........##2^3##............~.
z= 820 ..#####....#4^1^5#..####.....~~.
z= 780 ..#^^^^#....######.##^###...~...
z= 740 ..#^^^^##...##....###^##...~~~..
z= 700 ..###^^^#.........#.^##..~~~~...
z= 660 ....###^#..............~~~~~##..
z= 620 .......#..............~~~..###..
z= 580 .....#...............~~......##.
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
- **Claude** (private, only to you): Noted and matched: you keep the crest, then come down due south through x=520-660 from the north. Until the ring forces everyone in, our army stays south of the centre line, on Little Hill and the southern lowland. Our rider watches the centre from the south and will not go up toward the Heights. Happy to trade sightings: have you seen Blue's army on the West Plateau?
- **GPT** (public): Green holds the eastern central plain north of z600. Eye is moving east to (780,830), respecting White's requested separation. Neutrality and no first strike through t450 remain confirmed, with a full 25-second public notice before initiating hostilities afterward.
- **GPT** (private, only to you): Agreed. Eye is moving to (780,830) in the eastern lowland and will hold there. Our main force remains near (685,640). Please announce your descent a full turn ahead.
- **Gemini** (public): Blue holds West Plateau and maintains neutrality and our four-way ceasefire through t=450 with one full turn of public notice. Our descent lane into the western lowland (x=440-500, z=640-720) around t=275-300 is deconflicted with all neighbours.
- **Gemini** (private, only to you): Understood and agreed—x=520-660 is yours when you step down from Central Heights, and we will keep strictly west in x=440-500, z=640-720 when we leave West Plateau around t=275-300. All quiet on the western side.

## Your notes from last turn

T9 t=200. Pact LOCKED: neutral to all until t=450, then 1 full public turn of notice. If attacked: set that commander enemy the same turn, fight in place, no chase. HOLD all on crest. Ring 740 now, 660 at t=225. Burn if unmoved: crete 235m ~t439, thessaly 233m ~t440, volley 206m ~t461, hero 198m ~t475, sarissa 187m ~t488, odrysia 186m ~t489. T17 t=400: move ONLY crete+thessaly ~30m toward centre, stay on high ground N of cliff z~780. Main descent ~t470 via S corridor x=520-660; do not enter Claude's south lowland. GPT camel (730,830) ~72m E of odrysia: watch, do not chase; asked them east of x=760. GPT main claimed ~(680,630). Gemini holds West Plateau. End focus: heroes, elephants, champions, ranged. Losses 0.

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
