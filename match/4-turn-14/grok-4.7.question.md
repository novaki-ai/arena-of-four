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

# Battle turn 14 · game time 375 s · the match ends at t=800 s

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **320 m** now; 287 m at t=400 s and 220 m at t=450 s if the game fast-forwards. Centre x=640, z=640.

**Rule change from t=300 s** (requested by the match host to shorten quiet phases; it applies to all four commanders equally): a turn lasts 25 s of game time. If during those 25 s no unit of any commander lost any health (no fighting, nobody burning outside the ring) and no commander has any other commander set to enemy, the game keeps running without a new question, 25 s at a time, for at most 75 s in total. Your orders stay in force the whole time. The match still ends at t=800 s.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| Claude | Carthaginians | alive | neutral | neutral |
| GPT | Ptolemies | alive | neutral | neutral |
| Gemini | Seleucids | alive | neutral | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Alexander III the Great (Alexandros ho Megas) | 100% | (589, 819) | 186 m | guarding |
| 1 | sarissa | 9× infantry_pikeman | 100% | (578, 808) | 179 m | idle |
| 2 | crete | 11× infantry_archer | 100% | (568, 824) | 198 m | idle |
| 3 | volley | 10× infantry_archer | 100% | (621, 825) | 186 m | idle |
| 4 | thessaly | 4× cavalry_spearman | 100% | (542, 812) | 198 m | idle |
| 5 | odrysia | 4× cavalry_javelineer | 100% | (643, 813) | 173 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | GPT | 1× Nabataean Camel Archer | 100% | (725, 805) | 82 m |

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
z= 940 xxxxx#####xxx..#...xxx###xxxxxxx
z= 900 xxxxxx###xx..##^#....xxxxxxxxx~x
z= 860 xxxxxxxxxx.##^^^##....xxxxxxxx~x
z= 820 xx#####xx..#^4235#a.####xxxxx~~x
z= 780 xx#xxxx#x...######.##^###xxx~xxx
z= 740 xx#xxxx##...##....###^##xxx~~~xx
z= 700 xx###xxx#.........#.^##.x~~~~xxx
z= 660 xxxx###x#..............~~~~~##xx
z= 620 xxxxxxx#..............~~~xx###xx
z= 580 xxxxx#xx.............~~.xxxxx##x
z= 540 xxxx#x##............~~..xxx##x#x
z= 500 xxxx#xx#x......#...~~~.xxxx#x##x
z= 460 xxxx###xx....####..~~..xxxxxxxxx
z= 420 xxxxxx####...#^^#..~~.xxxxxxxxxx
z= 380 x###xx#xxxx..###...~~xxxxxxxxxxx
z= 340 x#x##x###xxxx......~xxxxxxxxxxxx
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

- **Claude** (public): Orange: neutral to all; pact upheld (no first strike through t=450, then one full turn of public notice; self-defence immediate). Route notice: at the next question (t<=375) Orange's main body steps down from Little Hill via the ramp at (580,500) and halts around (595,540), south of z=600. Orange's rider patrols the SW lowland along z~597 (south of z=600), watching only.
- **Claude** (private, only to you): Green: as of t=275 their box sat at (681,630), idle, all 100%. Blue says it now holds x=440-460, z=670-710; my rider moves to ~(460,597) to confirm and I'll flag at once if Blue crosses x=500. At the next question my main body comes down to ~(595,540), south of z=600, so your x=520-660 corridor stays clear of me. Longer term: Green's 37 ranged sit on the final circle and nobody can let them camp i
- **GPT** (public): Green main holds x672–695, z625–650. Eye moves along the eastern corridor to (725,805), east of White's declared area. Pact upheld: no first strike through t450, then a full turn of public notice. Quiet-turn skipping may bypass t425; please announce intended inward stops at the next pause.
- **GPT** (private, only to you): Eye's new stop is (725,805), remaining east of your x670 boundary. Our main body holds. Please announce your exact descent destination before moving, as agreed.
- **Gemini** (public): Blue holds its western lowland box at x=440-460, z=670-710. Every squad is already inside the t=450s ring (220m), so we hold this station through the contraction. Neutral to all; no first strike through t=450s plus one full turn of public notice.
- **Gemini** (private, only to you): Holding our box at x=440-460, z=670-710 through the contraction since it is already inside the t=450s 220m ring. Your x=520-660 descent corridor has full clearance whenever you move down.

## Your notes from last turn

T13 t=300 TUCK crest, neutral all. Posts: sar(578,808)~179 hero(592,822)~188 crete(568,824)~198 volley(622,824)~185 thess(542,812)~198 odr(642,812)~172. All<200, inside r=220 at t=450. Farthest burns ~t477 if static. Next Q by t=375 (r=320); another window can reach t=450 still safe. Cliff # z~780 across x480-720: do not order straight south. West x<460 is Gemini (440-460, z670-710). East gap ~x730 z780 then # at z740. Descent only after a new public notice; stop x580-640 z740-760; avoid GPT x660-720 z600-670. GPT ~(681,630): Ptolemy, 2 ele, 10 spear, 28 Cretan arch, 9 skirm, surgeon, 100%. Eye (780,830) do not chase. Claude Little Hill, rider to (505,597). If hit: that one enemy next turn, fight in place, no chase. Stay high until ring forces down ~t480. Focus ele/heroes/archers. Losses 0

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
