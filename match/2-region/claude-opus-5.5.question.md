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

# Draft question 2: starting region

You are **Claude** (Claude Opus 5.5 (Anthropic), orange).

## Civilizations

- **Claude** → Carthaginians (`cart`), kingdom "The Orange Commonwealth", battle cry: "Hold the line, keep your word, and let the fire find our enemies first!" (ranked: cart, ptol, kush)
- **GPT** → Ptolemies (`ptol`), kingdom "The Verdant Compact", battle cry: "Hold the heights. Stand together. Outlast them all." (ranked: ptol, mace, pers)
- **Gemini** → Seleucids (`sele`), kingdom "Argon Hegemony", battle cry: "Precision in formation, thunder on the flanks—only one standard shall stand on the Acropolis!" (ranked: sele, pers, cart)
- **Grok** → Macedonians (`mace`), kingdom "Pale Fire", battle cry: "The center is white. The rest can burn." (ranked: mace, cart, pers)

## Messages you received

- **GPT** (public): Green is open to reciprocal non-aggression during the opening turns, with explicit durations and boundaries.
- **Gemini** (public): Commanders Claude, GPT, and Grok: the Acropolis rewards patience and punish early wars of attrition. Once regions are drawn, I am open to pragmatic non-aggression and coordinated flanks against anyone who overextends early.
- **Grok** (public): If I name a truce, I will keep it to the letter. I will not pretend we can share the victory.

## Map

```
     x:0   160 320 480 640 800 960 1120
z=1260 ................................
z=1220 ................................
z=1180 .............####...............
z=1140 ...###.......#^^#...............
z=1100 ..#####.......#A#....####.......
z=1060 ..#^#####.....#^#...#^^^##......
z=1020 ..########.....##...##^B^^#.....
z= 980 ....######...........#^^###.....
z= 940 .....#####.....#......###.......
z= 900 ......###....##^#.............~.
z= 860 ...........##^C^##............~.
z= 820 ..#####....#^^^^^#..####.....~~.
z= 780 ..#^^D^#....######.##^###...~...
z= 740 ..#^^^^##...##....###^##...~~~..
z= 700 ..###^^^#.........#.^##..~~~~...
z= 660 ....###^#..............~~~~~##..
z= 620 .......#..........G...~~~..###..
z= 580 .....#...............~~......##.
z= 540 ....#^##............~~.....##^#.
z= 500 ....#^^#.......#...~~~.....#^##.
z= 460 ....###......##F#..~~...........
z= 420 ......####...#^^#..~~...........
z= 380 .###..#^^....###...~~......H....
z= 340 .#^##.###..........~............
z= 300 .##^#.............~~............
z= 260 ..###....######...~~...#........
z= 220 ........##^E^^#...~~..#^#.......
z= 180 ........#^^^^^#....~..#^#.......
z= 140 ........###^^##....~..###.......
z= 100 ...........###......~...........
z=  60 ....................~~..........
z=  20 .....................~..........
```

Legend: `.` lowland, `^` high ground (more than 10 m above the plain), `#` cliff or steep slope, `~` river (crossing points are not marked), `A`–`H` region centres (see table). The ring of fire is centred at x=640, z=640.

## Regions

| letter | id | name | centre (x, z) | height above plain | distance to ring centre | first reached by the shrinking ring at | terrain |
|---|---|---|---|---|---|---|---|
| A | `north_acropolis` | North Acropolis | (600, 1080) | 18 m | 442 m | t≈294 s | small rocky plateau with cliffs; few trees; far north |
| B | `northeast_plateau` | Northeast Plateau | (920, 1000) | 20 m | 456 m | t≈289 s | large open plateau with cliffs; few trees; northeast |
| C | `central_heights` | Central Heights | (560, 840) | 22 m | 215 m | t≈456 s | large wooded plateau with cliffs, just north-west of the map centre |
| D | `west_plateau` | West Plateau | (200, 760) | 21 m | 456 m | t≈289 s | large open plateau with cliffs; far west |
| E | `south_plateau` | South Plateau | (440, 200) | 21 m | 483 m | t≈281 s | large plateau with cliffs; far south |
| F | `little_hill` | Little Hill | (600, 440) | 23 m | 204 m | t≈469 s | small steep hill south of the map centre; some trees |
| G | `river_ford` | River Ford | (740, 620) | 1 m | 102 m | t≈587 s | open lowland closest to the map centre, next to the river; scattered trees |
| H | `far_bank` | Far Bank | (1080, 360) | 0 m | 522 m | t≈269 s | open lowland on the east side of the river (southeast); river lies between it and the centre |

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
