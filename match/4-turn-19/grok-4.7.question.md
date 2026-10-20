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

# Battle turn 19 · game time 575 s · the match ends at t=800 s

You are **Grok** (Grok 4.7 (xAI), white), civilization **Macedonians**, started in **Central Heights**.

Ring of fire: radius **112 m** now; 90 m at t=625 s and 90 m at t=675 s if the game fast-forwards. Centre x=640, z=640.

**Rule change from t=300 s** (requested by the match host to shorten quiet phases; it applies to all four commanders equally): a turn lasts 25 s of game time. If during those 25 s no unit of any commander lost any health (no fighting, nobody burning outside the ring) and no commander has any other commander set to enemy, the game keeps running without a new question, 25 s at a time, for at most 75 s in total. Your orders stay in force the whole time. The match still ends at t=800 s.

**Second rule change from t=525 s** (the match host asked to speed the match up again; it applies to all four commanders equally): each turn now lasts **50 s** of game time instead of 25 s. If during those 50 s no unit lost any health and no commander has another set to enemy, the game keeps running without a new question, 25 s at a time, for at most 100 s in total. The match still ends at t=800 s.

## Commanders

| commander | civilization | status | your stance to them | their stance to you |
|---|---|---|---|---|
| Claude | Carthaginians | alive | ally | neutral |
| GPT | Ptolemies | alive | enemy | enemy |
| Gemini | Seleucids | alive | ally | neutral |

## Your squads

| # | squad | units alive | avg health | centre (x, z) | distance to ring centre | doing |
|---|---|---|---|---|---|---|
| 0 | hero | 1× hero Alexander III the Great (Alexandros ho Megas) | 75% | (635, 680) | 40 m | individual.cheering |

Losses so far: you lost 0 units (0 during the last turn); your units killed 0 enemy units (0 during the last turn).

## Enemy units you can see now

| id | commander | units | avg health | centre (x, z) | distance to your nearest squad |
|---|---|---|---|---|---|
| a | Claude | 1× infantry_javelineer_iber_e, 8× Samnite Spearman, 14× Gallic Mercenary Swordsman, 17× Samnite Swordsman | 97% | (689, 629) | 74 m |
| b | Claude | 1× Italic Cavalry | 72% | (590, 629) | 68 m |
| c | GPT | 5× infantry_archer_e, 16× Cretan Mercenary Archer, 3× Mercenary Thureos Skirmisher, 1× hero Ptolemy I (Ptolemaios A' Sōter), 1× infantry_javelineer_e | 92% | (702, 714) | 76 m |
| d | Gemini | 1× hero Seleucus I (Seleukos A' Nikatōr), 3× Armored War Elephant, 12× Thracian Mercenary Swordsman, 7× Companion Cavalry, 10× Syrian Archer, 2× Surgeon | 100% | (643, 646) | 35 m |

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
z= 740 xx#xxxx##xxx##x..x###x##xxx~~~xx
z= 700 xx###xxx#xxxxx.0.c#xx##xx~~~~xxx
z= 660 xxxx###x#xxxx...d..xxxx~~~~~##xx
z= 620 xxxxxxx#xxxxx.b..a.xxx~~~xx###xx
z= 580 xxxxx#xxxxxxxx....xxx~~xxxxxx##x
z= 540 xxxx#x##xxxxxxx..xxx~~xxxxx##x#x
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

- **Claude** (public): Orange keeps its word: Green stays enemy, and all 36 Orange swords join our spears and elephant on Green's south and west faces now. No Orange strike on Blue or White while Green's army stands. Any strike on Orange gets Orange's whole army in return.
- **Claude** (private, only to you): Holding to it: only Green is enemy, and all 36 swords hit Green's south face now with the spears and elephant. I keep White at neutral in public so Blue doesn't read ahead; for our units it works the same as ally. Next: once Green's army is broken (eliminated or down to stragglers), Blue is the one untouched army: 3 armoured elephants, 7 Companions, 12 Thracian swords, 10 Syrian archers, Seleucus.
- **GPT** (public): Orange has 60 visible units at essentially full health. Blue and White: eliminating Green leaves that army intact for your next fight. I offer either of you mutual neutrality and cooperation against Orange; announce acceptance so we can coordinate the switch.
- **Gemini** (public): Blue anchors the west-center line at (650,644) with GPT set to enemy. Our elephants screen the center while our archers pour fire into Green's pocket at (696,639) alongside Orange from the south and White from the north. Neutral to Orange and White.
- **Gemini** (private, only to you): Green is down to 27 Cretan archers, 6 skirmishers, 1 towered elephant, 1 spearman, 1 surgeon, and Ptolemy at (696,639). Our elephants and 10 archers are firing into their west face from (646,644) while Orange's 60 units close from (649,608). Keep Alexander and your archers alive at range while Orange's infantry absorbs their fire.

## Your notes from last turn

T18 t525 EXTRACT. Alex 83, 7 arch 68, spear cav 89, jav cav 100. GPT b(696,639) 27 Cretan+tower ele 93%. Claude a(649,608) ~60 melee 100% S. Gemini c(632,648) 3 ele+7 comp+arch 100% W. Ally both, they neutral; GPT enemy. Rally hero(635,680) volley(638,698) standground scattered, cav guard volley. 40-58m from centre, safe at 90m ring. t575 r112 t625 r90. If GPT closes <60m: attack b, no chase x>710 or >75m from centre. If Gemini alone crosses z680: Gemini enemy, spear on comps, arrows on eles. If Claude alone crosses z680: Claude enemy. After GPT dies let them bleed; 1 notice before I strike. Later priority: surgeons, heroes, archers.

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
