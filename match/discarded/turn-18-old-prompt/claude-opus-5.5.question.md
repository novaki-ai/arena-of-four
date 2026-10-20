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

# Battle turn 18 · game time 525 s · the match ends at t=800 s

You are **Claude** (Claude Opus 5.5 (Anthropic), orange), civilization **Carthaginians**, started in **Little Hill**.

Ring of fire: radius **155 m** now; 133 m at t=550 s and 90 m at t=600 s if the game fast-forwards. Centre x=640, z=640.

**Rule change from t=300 s** (requested by the match host to shorten quiet phases; it applies to all four commanders equally): a turn lasts 25 s of game time. If during those 25 s no unit of any commander lost any health (no fighting, nobody burning outside the ring) and no commander has any other commander set to enemy, the game keeps running without a new question, 25 s at a time, for at most 75 s in total. Your orders stay in force the whole time. The match still ends at t=800 s.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| GPT | Ptolemies | alive | enemy | neutral |
| Gemini | Seleucids | alive | neutral | neutral |
| Grok | Macedonians | alive | neutral | ally |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Hannibal Barca (Ḥannibaʿal Baraq) | 100% | (670, 629) | 32 m | fighting |
| 1 | swords | 18× infantry_swordsman_gaul, 18× infantry_swordsman_ital | 100% | (630, 591) | 50 m | moving |
| 2 | spears | 20× infantry_spearman_ital | 100% | (683, 636) | 43 m | fighting |
| 3 | elephant | 1× champion_elephant | 100% | (683, 624) | 46 m | fighting |
| 4 | scout | 1× cavalry_spearman_ital | 72% | (598, 606) | 54 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | GPT | 1× hero Ptolemy I (Ptolemaios A' Sōter), 1× Towered War Elephant, 1× Mercenary Thureos Spearman, 27× Cretan Mercenary Archer, 6× Mercenary Thureos Skirmisher, 1× Surgeon | 93% | (696, 639) | 14 m |
| b | Gemini | 1× hero Seleucus I (Seleukos A' Nikatōr), 3× Armored War Elephant, 12× Thracian Mercenary Swordsman, 7× Companion Cavalry, 10× Syrian Archer, 2× Surgeon | 100% | (632, 648) | 43 m |
| c | Grok | 1× hero Alexander III the Great (Alexandros ho Megas), 7× Cretan Mercenary Archer, 1× Thessalian Lancer, 1× Odrysian Skirmish Cavalry | 75% | (661, 658) | 30 m |

## Last known positions of enemies you cannot see now

(none)

## Map now

```
     x:0   160 320 480 640 800 960 1120
z=1260 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1220 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1180 xxxxxxxxxxxxx####xxxxxxxxxxxxxxx
z=1140 xxx###xxxxxxx#xx#xxxxxxxxxxxxxxx
z=1100 xx#####xxxxxxx#x#xxxx####xxxxxxx
z=1060 xx#x#####xxxxx#x#xxx#xxx##xxxxxx
z=1020 xx########xxxxx##xxx##xxxx#xxxxx
z= 980 xxxx######xxxxxxxxxxx#xx###xxxxx
z= 940 xxxxx#####xxxxx#xxxxxx###xxxxxxx
z= 900 xxxxxx###xxxx##x#xxxxxxxxxxxxx~x
z= 860 xxxxxxxxxxx##xxx##xxxxxxxxxxxx~x
z= 820 xx#####xxxx#xxxxx#xx####xxxxx~~x
z= 780 xx#xxxx#xxxx######x##x###xxx~xxx
z= 740 xx#xxxx##xxx##....###x##xxx~~~xx
z= 700 xx###xxx#xxx......#.x##xx~~~~xxx
z= 660 xxxx###x#xxx...bc...xxx~~~~~##xx
z= 620 xxxxxxx#xxxx..4.0a..xx~~~xx###xx
z= 580 xxxxx#xxxxxx...1....x~~xxxxxx##x
z= 540 xxxx#x##xxxxx......x~~xxxxx##x#x
z= 500 xxxx#xx#xxxxxx.#..x~~~xxxxx#x##x
z= 460 xxxx###xxxxxx####xx~~xxxxxxxxxxx
z= 420 xxxxxx####xxx#xx#xx~~xxxxxxxxxxx
z= 380 x###xx#xxxxxx###xxx~~xxxxxxxxxxx
z= 340 x#x##x###xxxxxxxxxx~xxxxxxxxxxxx
z= 300 x##x#xxxxxxxxxxxxx~~xxxxxxxxxxxx
z= 260 xx###xxxx######xxx~~xxx#xxxxxxxx
z= 220 xxxxxxxx##xxxx#xxx~~xx#x#xxxxxxx
z= 180 xxxxxxxx#xxxxx#xxxx~xx#x#xxxxxxx
z= 140 xxxxxxxx###xx##xxxx~xx###xxxxxxx
z= 100 xxxxxxxxxxx###xxxxxx~xxxxxxxxxxx
z=  60 xxxxxxxxxxxxxxxxxxxx~~xxxxxxxxxx
z=  20 xxxxxxxxxxxxxxxxxxxxx~xxxxxxxxxx
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **GPT** (public): Green honors its announced notice: earliest initiation t525. These orders remain neutral and consolidate our army east of the centre, approximately x675–710, z625–655.
- **GPT** (private, only to you): White now visibly has Alexander, 9 Foot Companions, 19 Cretan archers, 4 lancers and 4 skirmish cavalry around (606,705). I propose mutual nonaggression while White lives, terminable with 25 seconds' notice. If you accept, we can coordinate pressure on White from south and east beginning at the next pause.
- **Gemini** (public): Blue sets Green to enemy at t=500 and advances east along z=636-658 into the central ring. Blue remains neutral to Orange and White and keeps all units strictly within our central-west corridor (z<=658).
- **Gemini** (private, only to you): Executing at t=500: GPT set to enemy, Claude and Grok kept neutral. Driving east along z=636-658 to (624-646, 636-658) with elephants and swordsmen in front to engage Green's western line as you strike (675,612) from the south. Coordinate focus fire on Green next pause.
- **Grok** (public): Committed. Green is enemy; Orange and Blue are allies. I close from the north on the force at (670,642): pikes and hero into it, archers and javelins firing. I will hit that group from its north side. Blue, take the west along z640-655. Orange, the south face is yours. No strikes among us while Green stands.
- **Grok** (private, only to you): Confirmed. Full commit this pause: only Green is enemy, you are ally. Pikes, hero, archers and javelin cavalry go onto (670,642) from the north. The southern Cretan screen and the main body are yours. I will not strike you while Green stands.

## Your notes from last turn

T17 t500: GPT->enemy as agreed. Swords+ele attack_move Green S Cretans (676,614); hero (625,586) w/ spears guarding = reserve/bodyguard; scout (598,606) watching Blue. NEXT: check Blue/White stances to GPT and to me. Both on GPT -> commit spears into GPT W/SW, hero ~40m behind fight. Either held back -> pull swords SW out of archer range, call it out publicly. Anyone sets me enemy -> all on them, hero inside spear blob. Ring t525 155, t550 133, t575 112, t600 90. After Green: steer Blue vs White, never ally, elephant off White pikes. White (GPT intel): Alexander, 9 Foot Comp, 17 Cretans, 4 Thess lancers, 4 Odrysian cav ~(600,708). Blue: hero, 3 armd ele, 12 swords, 7 cav, 10 archers, 2 surgeons ~(550,650).

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
