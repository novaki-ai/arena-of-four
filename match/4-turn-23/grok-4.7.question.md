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

# Battle turn 23 · game time 775 s · the match ends at t=800 s

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **90 m** now; 90 m at t=800 s. Centre x=640, z=640.

**Rule change from t=300 s** (requested by the match host to shorten quiet phases; it applies to all four commanders equally): a turn lasts 25 s of game time. If during those 25 s no unit of any commander lost any health (no fighting, nobody burning outside the ring) and no commander has any other commander set to enemy, the game keeps running without a new question, 25 s at a time, for at most 75 s in total. Your orders stay in force the whole time. The match still ends at t=800 s.

**Second rule change from t=525 s** (the match host asked to speed the match up again; it applies to all four commanders equally): each turn now lasts **50 s** of game time instead of 25 s. If during those 50 s no unit lost any health and no commander has another set to enemy, the game keeps running without a new question, 25 s at a time, for at most 100 s in total. The match still ends at t=800 s.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| Claude | Carthaginians | alive | ally | ally |
| GPT | Ptolemies | alive | enemy | enemy |
| Gemini | Seleucids | defeated | ally | ally |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Alexander III the Great (Alexandros ho Megas) | 75% | (579, 683) | 75 m | idle |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | Claude | 3× infantry_spearman_ital_e, 1× Italic Cavalry, 1× hero Hannibal Barca (Ḥannibaʿal Baraq) | 61% | (663, 697) | 85 m |

## Last known positions of enemies you cannot see now

- GPT: 9 units near (656, 718) (seen in turn 20)

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
z= 740 xx#xxxx##xxx##xxxx###x##xxx~~~xx
z= 700 xx###xxx#xxxxx0.a.#xx##xx~~~~xxx
z= 660 xxxx###x#xxxxx....xxxxx~~~~~##xx
z= 620 xxxxxxx#xxxxxx....xxxx~~~xx###xx
z= 580 xxxxx#xxxxxxxx....xxx~~xxxxxx##x
z= 540 xxxx#x##xxxxxxxxxxxx~~xxxxx##x#x
z= 500 xxxx#xx#xxxxxxx#xxx~~~xxxxx#x##x
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

- **Claude** (public): Orange: the score counts every living unit at price x health, hero or not. Blue's armored elephants, surgeon and Companion cav all count, so 'no hero, no win' is false. Green told me 'only Ptolemy, no archers' while fielding archers, so Orange now treats Green as hostile. Pact with White stands.
- **Claude** (private, only to you): Pact holds to the end. I will not strike Alexander unless Alexander strikes my units first. Hannibal is breaking Blue's wounded elephants now, then Green's archers. Keep Alexander clear of my units and I keep clear of yours.
- **GPT** (public): Green's current controllable roster is one Ptolemy at 30% health, with no archers. I am disengaging and setting everyone neutral. Reports of a large Green army are outdated.
- **GPT** (private, only to you): I am withdrawing south and staying away from your western position. Blue's surgeon accompanies two wounded elephants at (653,708). An opportunity to eliminate the surgeon could decide their fight with Orange.
- **Gemini** (public): Orange's math is outdated: our 3 Companion Cavalry died last turn killing 6 of Green's archers, and Seleucus is long dead. Blue is down to 2 elephants at 40% HP + 1 healer (~440 pts, no hero). Orange still has Hannibal (500) + 3 elite spearmen + 1 cav (~770 pts) at (707,655) and wins at t=800 unless everyone hits Orange in the final 25s!
- **Gemini** (private, only to you): Our 3 companions died fighting Green—we only have 2 elephants at 40% HP + 1 healer (~440 pts, no hero), which is LESS than your living Alexander (500 pts)! Once we finish Green's remnants here, only Orange (~770 pts with Hannibal + 3 spears + 1 cav at 707,655) beats you on the t=800 tiebreaker. Hit Orange in the final 25s!

## Your notes from last turn

T22 t725. Alex 75% (575,593)->move (579,683) standground. r~75, ~85m from GPT last (656,718). Ring fixed 90; stay r<=80. Saw Gemini at 73m on T21 and cannot see those spots now, so they moved; Gemini says eles+cav went N onto GPT. Only visible: Claude elite jav 100% (630,591), ally, do not touch. Diplo ally/ally/enemy so the peace-skip cannot jump to t=800. Value 375 if HP-scaled, else 500 flat; either way death is 0. T775: attack only a lone wounded hero or <=2 isolated units within ~45m with no ele/cav/archer ball; if archers close under 65m, retreat SW to (575,600); if Claude gives pact-break notice, run opposite. Never center, never r>85, never chase the ball.

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
