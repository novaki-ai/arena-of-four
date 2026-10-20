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

# Draft question 3: your army

You are **Gemini** (Gemini 4 Argon (Google), blue), civilization **Seleucids**, starting region **West Plateau (`west_plateau`)**.

Your budget: **4000 points**. At most 60 units plus one free hero.

## Draft results

Commanders were served in this order (highest bid first):

| commander | civilization | ranked regions | bid | received region | paid |
|---|---|---|---|---|---|
| Grok | Macedonians | central_heights, little_hill, river_ford | 800 | Central Heights (`central_heights`) | 800 |
| Claude | Carthaginians | central_heights, little_hill, west_plateau | 340 | Little Hill (`little_hill`) | 0 |
| GPT | Ptolemies | central_heights, little_hill, river_ford | 301 | River Ford (`river_ford`) | 0 |
| Gemini | Seleucids | little_hill, west_plateau, south_plateau | 0 | West Plateau (`west_plateau`) | 0 |

Regions:

| letter | id | name | centre (x, z) | height above plain | distance to ring centre | first reached by the shrinking ring at | terrain | taken by |
|---|---|---|---|---|---|---|---|---|
| A | `north_acropolis` | North Acropolis | (600, 1080) | 18 m | 442 m | t≈294 s | small rocky plateau with cliffs; few trees; far north |  |
| B | `northeast_plateau` | Northeast Plateau | (920, 1000) | 20 m | 456 m | t≈289 s | large open plateau with cliffs; few trees; northeast |  |
| C | `central_heights` | Central Heights | (560, 840) | 22 m | 215 m | t≈456 s | large wooded plateau with cliffs, just north-west of the map centre | Grok |
| D | `west_plateau` | West Plateau | (200, 760) | 21 m | 456 m | t≈289 s | large open plateau with cliffs; far west | Gemini |
| E | `south_plateau` | South Plateau | (440, 200) | 21 m | 483 m | t≈281 s | large plateau with cliffs; far south |  |
| F | `little_hill` | Little Hill | (600, 440) | 23 m | 204 m | t≈469 s | small steep hill south of the map centre; some trees | Claude |
| G | `river_ford` | River Ford | (740, 620) | 1 m | 102 m | t≈587 s | open lowland closest to the map centre, next to the river; scattered trees | GPT |
| H | `far_bank` | Far Bank | (1080, 360) | 0 m | 522 m | t≈269 s | open lowland on the east side of the river (southeast); river lies between it and the centre |  |

## Messages you received

- **Claude** (public): Orange (Carthage) offers all three of you the same terms: mutual non-aggression through t=300 s (end of turn 12), renewable. Accept by word or by stance, and we set neutral toward you as soon as we see it. We won't strike anyone who hasn't struck us. While the truce runs, any switch back to enemy gets a full turn of public notice. Hold the line, keep your word.
- **Claude** (private, only to you): Argon: agreed, early attrition only feeds whoever waits. Orange offers you the public terms: mutual non-aggression through t=300 s, renewable, one turn of notice before any change. I'm also open to the coordinated flank you described against whoever overextends first. Once regions are drawn, let's set a boundary.
- **GPT** (public): Claude, I accept your reciprocal truce until t=300 s. Gemini and Grok, I offer the same: neither side initiates attacks before t=300, with the agreement ending if either attacks. Peaceful movement toward the safe zone is permitted. Any extension beyond t=300 must be explicit.
- **Grok** (public): Pale Fire accepts a reciprocal truce until the ring reaches 420 m (t=300). I will not strike first at anyone who holds neutral or ally toward me until then. I came for the centre, not for an early scrap. After t=300 there is one winner.
- **Grok** (private, only to you): Argon Hegemony: agreed, no early attrition. Neutral or ally toward you until t=300 if you do the same. I will not punish a commander who keeps that word. Anyone who breaks the truce is the common problem.

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

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `A`–`H` region centres, the letter of your region is shown in brackets in the table above.

## Your catalogue (Seleucids)

Units (id → stats, price in points):

```json
{
 "cavalry_archer": {"name": "Dahae Horse Archer (Hippotoxotès Dahae)", "kind": ["Cavalry", "Ranged"], "cost": 150, "hp": 100, "armor": {"Crush": 15.0, "Hack": 2.0, "Pierce": 1.0}, "attack": [{"type": "ranged", "damage": {"Pierce": 7.5}, "every_s": 1.25, "range_m": 60, "splash": false}], "speed": 15.3, "vision_m": 80},
 "cavalry_javelineer": {"name": "Militia Cavalry (Hippakontistès Politès)", "kind": ["Cavalry", "Ranged"], "cost": 150, "hp": 100, "armor": {"Crush": 15.0, "Hack": 2.0, "Pierce": 1.0}, "attack": [{"type": "ranged", "damage": {"Pierce": 18.0}, "every_s": 1.5, "range_m": 30, "splash": false}], "speed": 16.2, "vision_m": 80},
 "cavalry_spearman_merc": {"name": "Companion Cavalry (Hippos Hetairike)", "kind": ["Cavalry", "Melee"], "cost": 110, "hp": 160, "armor": {"Crush": 15.0, "Hack": 3.0, "Pierce": 3.0}, "attack": [{"type": "melee", "damage": {"Hack": 6.6, "Pierce": 6.0}, "every_s": 1.25, "range_m": 4, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 1.75}]}], "speed": 18, "vision_m": 80, "note": "arrives at Advanced rank (stats shown are Advanced)"},
 "champion_cavalry": {"name": "Seleucid Cataphract (Seleukidón Kataphraktos)", "kind": ["Cavalry", "Champion", "Melee"], "cost": 340, "hp": 260, "armor": {"Crush": 20.0, "Hack": 8.0, "Pierce": 9.0}, "attack": [{"type": "melee", "damage": {"Hack": 12.0, "Pierce": 10.0}, "every_s": 1.25, "range_m": 7, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 1.75}]}], "speed": 14.4, "vision_m": 80},
 "champion_chariot": {"name": "Scythed Chariot (Drepanèphoros)", "kind": ["Cavalry", "Champion", "Chariot", "Ranged"], "cost": 400, "hp": 300, "armor": {"Crush": 20.0, "Hack": 1.0, "Pierce": 5.0}, "attack": [{"type": "ranged", "damage": {"Pierce": 15.0}, "every_s": 1.25, "range_m": 60, "splash": false}], "speed": 17, "vision_m": 80},
 "champion_elephant": {"name": "Armored War Elephant (Thorakisménos Eléphantos)", "kind": ["Champion", "Elephant", "Melee"], "cost": 550, "hp": 1100, "armor": {"Crush": 20.0, "Hack": 5.0, "Pierce": 7.0}, "attack": [{"type": "melee", "damage": {"Crush": 49.5, "Hack": 33.0}, "every_s": 1.5, "range_m": 5, "splash": true}], "speed": 9, "vision_m": 100},
 "champion_infantry_pikeman": {"name": "Silver Shield (Argyraspis)", "kind": ["Infantry", "Champion", "Melee"], "cost": 220, "hp": 200, "armor": {"Crush": 20.0, "Hack": 8.0, "Pierce": 8.0}, "attack": [{"type": "melee", "damage": {"Hack": 8.0, "Pierce": 15.0}, "every_s": 2.0, "range_m": 8, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 2.5}]}], "speed": 8.5, "vision_m": 80},
 "champion_infantry_swordsman": {"name": "Romanized Heavy Swordsman (Thorakitès Rhomaïkós)", "kind": ["Infantry", "Champion", "Melee"], "cost": 220, "hp": 200, "armor": {"Crush": 20.0, "Hack": 6.0, "Pierce": 6.0}, "attack": [{"type": "melee", "damage": {"Hack": 16.0}, "every_s": 0.75, "range_m": 3, "splash": false}], "speed": 9.5, "vision_m": 80},
 "infantry_archer_merc": {"name": "Syrian Archer (Toxótēs Syrías)", "kind": ["Infantry", "Ranged"], "cost": 60, "hp": 50, "armor": {"Crush": 10.0, "Hack": 1.0, "Pierce": 1.0}, "attack": [{"type": "ranged", "damage": {"Pierce": 7.9}, "every_s": 1.25, "range_m": 60, "splash": false}], "speed": 10.3, "vision_m": 80, "note": "arrives at Advanced rank (stats shown are Advanced)"},
 "infantry_javelineer": {"name": "Arab Javelineer (Pezakontistès Aravikós)", "kind": ["Infantry", "Ranged"], "cost": 100, "hp": 50, "armor": {"Crush": 10.0, "Hack": 1.0, "Pierce": 1.0}, "attack": [{"type": "ranged", "damage": {"Pierce": 16.0}, "every_s": 1.5, "range_m": 30, "splash": false}], "speed": 11.4, "vision_m": 80},
 "infantry_pikeman": {"name": "Phalangite (Phalangitēs)", "kind": ["Infantry", "Melee"], "cost": 100, "hp": 100, "armor": {"Crush": 15.0, "Hack": 5.0, "Pierce": 5.0}, "attack": [{"type": "melee", "damage": {"Hack": 4.0, "Pierce": 7.5}, "every_s": 2.0, "range_m": 8, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 2.5}]}], "speed": 8.5, "vision_m": 80},
 "infantry_spearman": {"name": "Militia Thureos Spearman (Thureophóros Politès)", "kind": ["Infantry", "Melee"], "cost": 100, "hp": 100, "armor": {"Crush": 15.0, "Hack": 3.0, "Pierce": 3.0}, "attack": [{"type": "melee", "damage": {"Hack": 4.5, "Pierce": 4.0}, "every_s": 1.0, "range_m": 4, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 2.5}]}], "speed": 9.5, "vision_m": 80},
 "infantry_swordsman_merc": {"name": "Thracian Mercenary Swordsman (Rhomphaiaphoros Thrakikós)", "kind": ["Infantry", "Melee"], "cost": 60, "hp": 100, "armor": {"Crush": 15.0, "Hack": 3.0, "Pierce": 3.0}, "attack": [{"type": "melee", "damage": {"Hack": 8.8}, "every_s": 0.75, "range_m": 3, "splash": false}], "speed": 9.5, "vision_m": 80, "note": "arrives at Advanced rank (stats shown are Advanced)"},
 "support_healer": {"name": "Surgeon (Iatros)", "kind": ["Healer"], "cost": 130, "hp": 85, "armor": {"Crush": 1.0, "Hack": 1.0, "Pierce": 1.0}, "attack": [], "speed": 9, "vision_m": 30, "heals": {"range": 12, "hp": 5, "rate": 2000}}
}
```

Heroes (choose exactly one; free):

```json
{
 "hero_antiochus_iii": {"name": "Antiochus III the Great (Antiokhos G' Megas)", "kind": ["Cavalry", "Melee"], "hp": 1200, "armor": {"Crush": 25.0, "Hack": 11.0, "Pierce": 10.0}, "attack": [{"type": "melee", "damage": {"Hack": 16.0, "Pierce": 12.0}, "every_s": 1.25, "range_m": 4, "splash": false, "bonus_vs": [{"classes": "Cavalry", "multiplier": 1.75}]}], "speed": 18, "auras": [{"name": "Inspired Defense", "effect": "When garrisoned, the Structure has +1 capture points garrison regeneration rate."}, {"name": "Ilarchès", "effect": "Cavalry +2 crush, hack, pierce resistance."}]},
 "hero_antiochus_iv": {"name": "Antiochus IV (Antiokhos D' Epiphanēs)", "kind": ["Cavalry", "Melee"], "hp": 1200, "armor": {"Crush": 25.0, "Hack": 11.0, "Pierce": 9.0}, "attack": [{"type": "melee", "damage": {"Hack": 26.0}, "every_s": 0.75, "range_m": 4, "splash": false}], "speed": 18, "auras": [{"name": "Inspired Defense", "effect": "When garrisoned, the Structure has +1 capture points garrison regeneration rate."}, {"name": "Renowned Conqueror", "effect": "Enemy Structures, Ships, and Siege Engines −20% health."}]},
 "hero_seleucus_i": {"name": "Seleucus I (Seleukos A' Nikatōr)", "kind": ["Elephant", "Melee"], "hp": 1500, "armor": {"Crush": 25.0, "Hack": 10.0, "Pierce": 10.0}, "attack": [{"type": "melee", "damage": {"Crush": 90.0, "Hack": 60.0}, "every_s": 1.5, "range_m": 5, "splash": true}], "speed": 9, "auras": [{"name": "Inspired Defense", "effect": "When garrisoned, the Structure has +1 capture points garrison regeneration rate."}, {"name": "Zooiarchos", "effect": "Champion Elephants +20% melee attack damage, +20% movement speed."}]}
}
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
