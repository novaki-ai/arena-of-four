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

# Battle turn 6 of 32 · game time 125 s

You are **Claude** (Claude Opus 5.5 (Anthropic), orange), civilization **Carthaginians**, started in **Little Hill**.

Ring of fire: radius **900 m** now; it will be **900 m** when this turn ends (t=150 s). Centre x=640, z=640.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| GPT | Ptolemies | alive | neutral | neutral |
| Gemini | Seleucids | alive | neutral | neutral |
| Grok | Macedonians | alive | neutral | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Hannibal Barca (Ḥannibaʿal Baraq) | 100% | (586, 449) | 198 m | idle |
| 1 | swords | 18× infantry_swordsman_gaul, 18× infantry_swordsman_ital, 1× infantry_javelineer_iber | 100% | (598, 434) | 210 m | idle |
| 2 | spears | 20× infantry_spearman_ital | 100% | (601, 453) | 191 m | idle |
| 3 | elephant | 1× champion_elephant | 100% | (609, 429) | 213 m | idle |
| 4 | scout | 1× cavalry_spearman_ital | 100% | (655, 575) | 67 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | GPT | 1× hero Ptolemy I (Ptolemaios A' Sōter), 2× Towered War Elephant, 7× Mercenary Thureos Spearman, 28× Cretan Mercenary Archer, 9× Mercenary Thureos Skirmisher, 1× Surgeon | 100% | (680, 626) | 57 m |

## Last known positions of enemies you cannot see now

(none)

## Map now

```
     x:0   160 320 480 640 800 960 1120
z=1260 ................................
z=1220 ................................
z=1180 .............####...............
z=1140 ...###.......#^^#...............
z=1100 ..#####.......#^#....####.......
z=1060 ..#^#####.....#^#...#^^^##......
z=1020 ..########.....##...##^^^^#.....
z= 980 ....######...........#^^###.....
z= 940 .....#####.....#......###.......
z= 900 ......###....##^#.............~.
z= 860 ...........##^^^##............~.
z= 820 ..#####....#^^^^^#..####.....~~.
z= 780 ..#^^^^#....######.##^###...~...
z= 740 ..#^^^^##...##....###^##...~~~..
z= 700 ..###^^^#.........#.^##..~~~~...
z= 660 ....###^#..............~~~~~##..
z= 620 .......#........a.....~~~..###..
z= 580 .....#..........4....~~......##.
z= 540 ....#^##............~~.....##^#.
z= 500 ....#^^#.......#...~~~.....#^##.
z= 460 ....###......#02#..~~...........
z= 420 ......####...#13#..~~...........
z= 380 .###..#^^....###...~~...........
z= 340 .#^##.###..........~............
z= 300 .##^#.............~~............
z= 260 ..###....######...~~...#........
z= 220 ........##^^^^#...~~..#^#.......
z= 180 ........#^^^^^#....~..#^#.......
z= 140 ........###^^##....~..###.......
z= 100 ...........###......~...........
z=  60 ....................~~..........
z=  20 .....................~..........
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **GPT** (public): Green remains neutral. We accept Blue's extension: mutual no-first-strike through t450, then rolling neutrality with one full 25-second public turn of notice before any stance change. Same terms with Orange and White. Main army holds the central lowland; our lone rider stays off occupied heights.
- **GPT** (private, only to you): Your scout is visible at (670,600); neutrality holds. We see one White skirmish cavalry at (664,828), but no White main force. This sighting alone does not indicate an advance.
- **Gemini** (public): Blue remains neutral to all and stays on West Plateau with no scouts out. We formally confirm extending our mutual no-first-strike truce to t=450 (with rolling one-full-public-turn notice thereafter) with Orange and White, and we offer Green the same extension from t=300 to t=450 on identical terms.
- **Gemini** (private, only to you): Confirmed: our mutual truce is now extended to t=450 with rolling one-full-public-turn notice after. Respecting unit composition privacy makes complete sense; position and movement alerts are all we need. Green told me their scout is moving along the eastern lowland of Central Heights while White holds the crest.
- **Grok** (public): White: neutral to all. No first strike before t=450, then one full public turn of notice. If I am attacked I set that commander enemy the same turn. I see Green's lone scout at the announced lowland point east of the Heights; I will not engage it while it stays off the crest and holds fire. Blue and Green: the offer to extend our truce to t=450 on those same terms is open.
- **Grok** (private, only to you): Your southern watch is welcome. I see one Green camel archer at (740,841), lowland east of the crest, matching their waypoint. No other contact. I will not engage it. Truce to t=450 stands; I will warn you if anything leaves the Heights toward Little Hill.

## Your notes from last turn

T5: Gemini+Grok took t450 ext. ALL 3: no 1st strike to t450, then rolling 1 public-turn notice; attacks answered same turn. Gemini: x=480 line. Grok: scout ok in lowland off crest. GPT whole army T5 @(698,628) on final ring: Ptolemy I, 2 towered eleph, 10 thureos spear, 28 Cretan archer, 9 skirm, surgeon (51u). Gemini+Grok trade intel (bloc?); GPT = my N shield. Scout ->(655,575). Offered GPT deconfliction line pre-descent. Share positions not compositions; GPT pos not shared. Never ally (Hannibal aura). Ring: 900-3.2(t-150) to t300; 420-1.33(t-300) to t450; 220-0.87(t-450) to t600. Hill exits ~t458-483, N edge z480 ~t510. Descend T18(t425)/T19 to ~(630,560). Gemini off plateau ~t290-360, Grok off heights ~t440. Goal: others bleed, I stay fresh.

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
