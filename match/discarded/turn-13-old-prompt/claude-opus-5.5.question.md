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

# Battle turn 13 of 32 · game time 300 s

You are **Claude** (Claude Opus 5.5 (Anthropic), orange), civilization **Carthaginians**, started in **Little Hill**.

Ring of fire: radius **420 m** now; it will be **387 m** when this turn ends (t=325 s). Centre x=640, z=640.

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
| 4 | scout | 1× cavalry_spearman_ital | 100% | (506, 597) | 141 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

(none)

## Last known positions of enemies you cannot see now

- GPT: 51 units near (681, 630) (seen in turn 12)

## Map now

```
     x:0   160 320 480 640 800 960 1120
z=1260 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1220 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
z=1180 xxxxxxxxxxxxx####xxxxxxxxxxxxxxx
z=1140 xxx###xxxxxxx#xx#xxxxxxxxxxxxxxx
z=1100 xx#####xxxxxxx#x#xxxx####xxxxxxx
z=1060 xx#x#####xxxxx#x#xxx#xxx##xxxxxx
z=1020 xx########xx...##...##xxxx#xxxxx
z= 980 xxxx######...........#xx###xxxxx
z= 940 xxxxx#####.....#......###xxxxxxx
z= 900 xxxxxx###....##^#.......xxxxxx~x
z= 860 xxxxxxx....##^^^##.......xxxxx~x
z= 820 xx#####....#^^^^^#..####.xxxx~~x
z= 780 xx#xxx^#....######.##^###.xx~xxx
z= 740 xx#xxx^##...##....###^##..x~~~xx
z= 700 xx###x^^#.........#.^##..~~~~xxx
z= 660 xxxx###^#..............~~~~~##xx
z= 620 xxxxxx.#..............~~~.x###xx
z= 580 xxxxx#......4........~~...xxx##x
z= 540 xxxx#x##............~~....x##x#x
z= 500 xxxx#x^#.......#...~~~....x#x##x
z= 460 xxxx###......#02#..~~....xxxxxxx
z= 420 xxxxxx####...#13#..~~....xxxxxxx
z= 380 x###xx#x^....###...~~...xxxxxxxx
z= 340 x#x##x###..........~...xxxxxxxxx
z= 300 x##x#xxxxx........~~..xxxxxxxxxx
z= 260 xx###xxxx######...~~xxx#xxxxxxxx
z= 220 xxxxxxxx##xxxx#xxx~~xx#x#xxxxxxx
z= 180 xxxxxxxx#xxxxx#xxxx~xx#x#xxxxxxx
z= 140 xxxxxxxx###xx##xxxx~xx###xxxxxxx
z= 100 xxxxxxxxxxx###xxxxxx~xxxxxxxxxxx
z=  60 xxxxxxxxxxxxxxxxxxxx~~xxxxxxxxxx
z=  20 xxxxxxxxxxxxxxxxxxxxx~xxxxxxxxxx
```

Legend: `.` lowland, `^` high ground, `#` cliff or steep slope, `~` river, `x` outside the ring right now, digits = your squads (number in the squad table), lowercase letters = enemy groups you can see (ids in the enemy table).

## Messages you received

- **GPT** (public): Green holds its central lowland position; Eye holds at (780,830). Neutral to all. Pact unchanged: no first strike through t=450, then one full turn of public notice before hostilities; immediate self-defence reserved. Please announce inward routes and destinations before approaching another army.
- **GPT** (private, only to you): Your rider is visible at (678,584); the z=600 boundary remains respected. No new White sightings. We can revisit inward spacing at t=425 as agreed.
- **Gemini** (public): Blue has completed its step-down from West Plateau and holds its western lowland position (x=440-460, z=670-710). Neutral to all; pact upheld (no first strike through t=450, then one full turn of public notice).
- **Gemini** (private, only to you): Thanks for the detailed scout report on Green's archer-heavy box at (681,630). With 28 Cretan Archers + 9 Skirmishers behind 2 Elephants and 10 Spearmen, Green cannot be allowed to camp the final 90m circle unchallenged. We are now off the plateau at x=440-460, z=670-710. Let's keep sharing intel and coordinate a joint plan before t=450.
- **Grok** (public): White holds the Central Heights crest this turn. All squads stay north of z=800 between x=500 and x=670. No descent. Pact stands: neutral to all, no first strike through t=450, then one full turn of public notice; self-defence is immediate. Any later descent will be announced one turn ahead, with a stopping point inside x=520-660 that stays clear of Green at x672-695 and Blue west of x=450.
- **Grok** (private, only to you): Thanks for the sighting. Holding the crest north of z=800 this turn; no descent. I will stay east of x=500 so your rider along z~590 and my corridor do not meet. Please ping me if Green steps north of z=660 or Blue crosses x=500. Pact holds.

## Your notes from last turn

T12 t275 quiet, all neutral. GPT (681,630) comp unchanged, 100%, idle. Rider -> (505,597) to see Gemini descent (box x430-460 z660-710), prob only SE edge; it had been 46m from GPT archers, too close. T13: log Gemini comp, relay to Grok. T14: rider back E ~(630,552), ~95m from GPT centre. Pact: no 1st strike to t450 + 1 public turn notice -> earliest attack t475; honor it. z600 line w/ GPT (me S), revisit t425. Grok must leave Heights by ~t440 via S corridor x520-660 -> lands next to GPT: want Grok/Gemini to hit GPT first. Plateau x560-640 z400-480, only exit ramp (580,500). N edge (590,470)=177m -> ring-safe to ~t497. Exit T18-19, out before t475. Ring t300 r420, t425 r253, t450 r220, t475 r198, t500 r177, t550 r133, t600 r90. Never ally.

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
