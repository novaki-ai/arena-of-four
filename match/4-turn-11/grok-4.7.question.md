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

# Battle turn 11 of 32 · game time 250 s

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **580 m** now; it will be **500 m** when this turn ends (t=275 s). Centre x=640, z=640.

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
z=1260 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1220 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1180 xxxxxxxxxxx..####....xxxxxxxxxxx
z=1140 xxx###xxx....#^^#......xxxxxxxxx
z=1100 xx#####.......#^#....####xxxxxxx
z=1060 xx#x#####.....#^#...#^^^##xxxxxx
z=1020 xx########.....##...##^^^^#xxxxx
z= 980 xxxx######...........#^^###.xxxx
z= 940 xxxx.#####.....#......###...xxxx
z= 900 xxx...###....##^#............x~x
z= 860 xxx........##2^3##...........x~x
z= 820 xx#####....#4^1^5#..####.....~~x
z= 780 xx#^^^^#....######.##^###...~.xx
z= 740 xx#^^^^##...##....###^##...~~~xx
z= 700 xx###^^^#.........#.^##..~~~~.xx
z= 660 xx..###^#..............~~~~~##xx
z= 620 xx.....#..............~~~..###xx
z= 580 xx...#...............~~......##x
z= 540 xx..#^##............~~.....##^#x
z= 500 xx..#^^#.......#...~~~.....#^##x
z= 460 xx..###......####..~~.........xx
z= 420 xxx...####...#^^#..~~........xxx
z= 380 x###..#^^....###...~~........xxx
z= 340 x#x##.###..........~........xxxx
z= 300 x##x#.............~~........xxxx
z= 260 xx###....######...~~...#...xxxxx
z= 220 xxxxxx..##^^^^#...~~..#^#.xxxxxx
z= 180 xxxxxxx.#^^^^^#....~..#^#xxxxxxx
z= 140 xxxxxxxx###^^##....~..###xxxxxxx
z= 100 xxxxxxxxxxx###......~xxxxxxxxxxx
z=  60 xxxxxxxxxxxxxxxxxxxx~~xxxxxxxxxx
z=  20 xxxxxxxxxxxxxxxxxxxxx~xxxxxxxxxx
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **Claude** (public): Orange: quiet again. Army stays on Little Hill, rider stays south of z600. Pact reaffirmed: neutral to all, no first strike through t450, then a full public turn of notice before any hostilities. We are not climbing toward the Heights or the West Plateau, and nothing of ours is in Blue's x440-500 lane.
- **Claude** (private, only to you): Nothing is climbing toward the Heights from the south; my army is still on Little Hill and won't head your way. From my rider: Green's main body, hero included, is still parked at ~(680,620), right at the foot of your x520-660 corridor, and it is archer-heavy. No sign of it moving north yet; I'll tell you at once if that changes. Anything you can see of Blue on the West Plateau?
- **GPT** (public): Green holds the central lowland. Pact unchanged: no first strike through t=450, then a full turn of public notice before initiating hostility. Blue's western descent lane remains clear of Green. Our eastern scout holds position off the crest.
- **GPT** (private, only to you): Eye now holds at (780,830), east of your requested x=760 boundary. No movement toward the crest.
- **Gemini** (public): Blue remains stationary on the West Plateau at (255-280, 695-729). We uphold the four-way non-aggression pact through t=450 with one full turn of public notice thereafter. We will announce publicly before beginning our step down into the western lowland (x=440-500, z=640-720) ahead of the t=300 ring contraction.
- **Gemini** (private, only to you): Confirmed, Blue remains stationary on the West Plateau this turn and upholds the pact through t=450. As the ring approaches 420m around t=275-300, we will announce before stepping down into the western lowland (x=440-500, z=640-720), staying clear of the Central Heights.

## Your notes from last turn

T10 t=225 HOLD crest. Neutral all. Ring 660→580. Ranges: crete 235 ~burn t439, thessaly 233 t440, volley 206 t461, hero 198 t475, sarissa 187 t488, odrysia 186 t489. No LOS. GPT scout last (730,830)→(780,830); main claimed (685,640) east of lane. Claude rider claimed (648,566) unseen, army south of centre. Gemini West Plateau, drop t=275-300 x=440-500 z=640-720. Pact through t=450 then 1 public turn notice. If hit: that one enemy same turn, fight in place, no chase. T17 t=400 nudge crete+thessaly ~30m inward, stay high, N of cliff z~780. Descent ~t=470 x=520-660 only, announce 1 turn prior. Avoid Claude south and Gemini west. Focus: heroes, elephants, champions, ranged. Losses 0.

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
