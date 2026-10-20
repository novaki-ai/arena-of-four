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

# Battle turn 16 · game time 475 s · the match ends at t=800 s

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **198 m** now; 177 m at t=500 s and 133 m at t=550 s if the game fast-forwards. Centre x=640, z=640.

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
| 0 | hero | 1× hero Alexander III the Great (Alexandros ho Megas) | 100% | (601, 717) | 86 m | guarding |
| 1 | sarissa | 9× infantry_pikeman | 100% | (600, 707) | 78 m | idle |
| 2 | crete | 11× infantry_archer | 100% | (592, 706) | 82 m | idle |
| 3 | volley | 10× infantry_archer | 100% | (610, 712) | 78 m | idle |
| 4 | thessaly | 4× cavalry_spearman | 100% | (586, 698) | 79 m | idle |
| 5 | odrysia | 4× cavalry_javelineer | 100% | (604, 710) | 79 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | GPT | 1× Nabataean Camel Archer, 2× Towered War Elephant | 100% | (657, 645) | 82 m |
| b | Gemini | 1× hero Seleucus I (Seleukos A' Nikatōr), 3× Armored War Elephant, 12× Thracian Mercenary Swordsman, 7× Companion Cavalry, 10× Syrian Archer, 2× Surgeon | 100% | (542, 655) | 61 m |

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
z= 820 xx#####xxxx#xx^^^#xx####xxxxx~~x
z= 780 xx#xxxx#xxxx######.##x###xxx~xxx
z= 740 xx#xxxx##xxx##....###x##xxx~~~xx
z= 700 xx###xxx#xx...45..#.^##xx~~~~xxx
z= 660 xxxx###x#xx..b..a....xx~~~~~##xx
z= 620 xxxxxxx#xxx..........x~~~xx###xx
z= 580 xxxxx#xxxxx..........~~xxxxxx##x
z= 540 xxxx#x##xxxx........~~xxxxx##x#x
z= 500 xxxx#xx#xxxx...#...~~~xxxxx#x##x
z= 460 xxxx###xxxxxx####.x~~xxxxxxxxxxx
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

- **Claude** (public): Orange notice under the pact: from one full 25-s turn after this notice is delivered (t500 at the earliest), Orange may engage any commander. Until then Orange stays neutral to all, no first strike; self-defence immediate. Army stays at its declared stop near (595,540) this turn.
- **Claude** (private, only to you): Confirming; my generic notice is out. My rider steps a few metres past the ring edge this turn, which blocks the quiet-skip, so there should be a pause at t475 where the t450 notices land. Contact window t500, not t475. You on Green's north face, Blue west, me south; I see 10 Cretans at ~(675,612) on their south face. What do you see on their north? If the t475 pause does not happen, the window is
- **GPT** (public): Green remains neutral. Main army holds x672-695, z625-650. My lone horse archer moves to (628,635) for observation in the central corridor, outside everyone's announced boxes. No attack notice issued. Our pact continues: a full 25 seconds of public notice before a first strike; immediate self-defence reserved.
- **GPT** (private, only to you): My main army holds its existing eastern position. The lone scout moves southwest to (628,635), south of your announced band. Neutrality and the notice agreement remain in force.
- **Gemini** (public): Blue: stepping east into our declared x=535-555, z=642-660 station inside the t=525 ring (155m). Neutral to all this turn. With t=450 reached and the ring contracting toward 90m, this serves as our formal one-turn public notice that truce restrictions expire after this turn; full freedom of maneuver and engagement is reserved from the next pause.
- **Gemini** (private, only to you): Box respected. We are stepping into x=536-552, z=642-660 now, keeping z<=660 (at least 40m south of your z=700 line) and holding neutral. We are posting our generic post-t450 public notice this turn. Proposal for the next pause: 3-way convergence on Green at (680,640) pinned against the eastern cliff—you push south from (610,720), Orange pushes north from (600,545), and we push east along z=640-65

## Your notes from last turn

T15 t=450 NOTICE only, all neutral. Halts: sar(600,706) r77 dEle~90, crete(592,706) r79, volley(610,712) r78 dEle~86, thess(586,698) r79, odr(604,710) r79, hero guards sar. Box x575-625 z690-720. GPT ele a(669,649), camp ~(681,630) x672-695 z625-650; est 2 ele, 10 spear, 28 arch, 9 skirm, surgeon. Gemini next x520-560 z640-680, 6 swords (460,670). Claude ~(595,540); rider (460,597) r~185 burns ~t490. Next pause: t=475 if anyone sets enemy, else t=500 if rider burns, else t=525. Then if pact holds set GPT enemy: pikes+hero on elephants, archers elephants then ranged, cav screen, no chase, stay x<=640. If hit first: that one only, fight in place. Ring 198@475 176@500 155@525 90@600. Our r<=82 fits inside r90. Do not chase eye (710,680).

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
