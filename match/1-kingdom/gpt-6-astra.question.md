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

# Draft question 1: your kingdom

You are **GPT**: GPT-6 Astra (OpenAI). Your units will be green.

Available civilizations. Everybody gets the same budget; the differences are the unit types and heroes. Numbers in brackets are unit prices in points. You will see full statistics after the region draft.

- `athen` **Athenians**: units: Cretan Mercenary Archer (60), Thracian Peltast (60), Cretan Mercenary Archer (60), Athenian Slinger Militia (100), Athenian Hoplite (100), Surgeon (130), Cavalry Javelineer (150), Greek Cavalry (150), Athenian Marine (160), City Guard (220), Scythian Archer (220). Heroes: Hippocrates, Iphicrates, Pericles, Themistocles, Xenophon.
- `brit` **Britons**: units: Infantry Javelineer (100), Celtic Slinger (100), Celtic Spearman (100), Druid (130), Raiding Cavalry (150), Celtic Cavalry (150), Brythonic Champion (220), Celtic Chariot (400). Heroes: Boudicca, Caratacus, Cunobeline.
- `cart` **Carthaginians**: units: Samnite Spearman (60), Gallic Mercenary Swordsman (60), Samnite Swordsman (60), Balearic Slinger (75), Iberian Mercenary Skirmisher (80), Mauritanian Archer (100), Libyan Spearman (100), Italic Cavalry (110), Gallic Mercenary Cavalry (110), Iberian Heavy Cavalry (110), Healer (130), Numidian Cavalry (150), Sacred Band Infantry (220), Sacred Band Pikeman (220), Sacred Band Cavalry (330), North African War Elephant (450). Heroes: Hamilcar Barca, Hannibal Barca, Maharbal.
- `gaul` **Gauls**: units: Infantry Javelineer (100), Slinger (100), Spearman (100), Druid (130), Cavalry Javelineer (150), Cavalry Swordsman (150), Naked Fanatic (220), Champion Swordsman (220), Champion Infantry Trumpeter (300), Gallic Noble Cavalry (330). Heroes: Brennus, Vercingetorix, Viridomarus.
- `germ` **Germans**: units: Cimbrian Clubman (100), Ambronian Skirmisher (100), Celto-Germanic Slinger (100), Cimbrian Spearman (100), Teutonic Swordsman (100), Germanic Priest (130), Tigurinian Light Cavalry (150), Cimbrian Spear Cavalry (150), Cimbri Raider (220), Seeress (250), Cimbrian Noble Cavalry (330). Heroes: Hero Cavalry Swordsman, Hero Cavalry Spearman, Hero Swordsman.
- `han` **Han Chinese**: units: Archer (100), Infantry Crossbowman (100), Halberdier (100), Spearman (100), Swordsman (100), Healer (130), Cavalry Archer (150), Cavalry Crossbowman (150), Cavalry Spearman (150), Cavalry Swordsman (150), Palace Guard Archer (220), Champion Infantry Crossbowman (220), Tiger Guard Spearman (220), Youzhou Cavalry (330), Han War Chariot (400). Heroes: Hero Pikeman, Hero Swordsman, Hero Infantry Crossbowman.
- `iber` **Iberians**: units: Infantry Javelineer (100), Slinger (100), Spearman (100), Swordsman (100), Priestess of Ataekina (130), Cavalry Javelineer (150), Cavalry Spearman (150), Champion Swordsman (220), Champion Cavalry Javelineer (330). Heroes: Hero Swordsman, Hero Cavalry Spearman, Hero Swordsman.
- `kush` **Kushites**: units: Noba Skirmisher (60), Noba Maceman (60), Nubian Archer (100), Meroitic Pikeman (100), Nubian Spearman (100), Meroitic Swordsman (100), Blemmye Desert Raider (110), Priest (130), Napatan Light Cavalry (150), Meroitic Heavy Cavalry (150), Napatan Temple Guard (220), Meroitic Temple Guard (220), Noble Archer (220), Noble Cavalry (330), Meroitic War Elephant (450). Heroes: Amanirenas, Arakamani, Harsiotef, Nastasen.
- `mace` **Macedonians**: units: Cretan Mercenary Archer (60), Rhodian Slinger (60), Agrianian Peltast (100), Foot Companion (100), Odrysian Skirmish Cavalry (110), Surgeon (130), Thessalian Lancer (150), Champion Infantry Crossbowman (220), Macedonian Shield Bearer (220), Thracian Black Cloak (220), Companion Cavalry (330). Heroes: Alexander III the Great, Craterus, Demetrius I the Besieger, Philip II of Macedon, Pyrrhus I of Epirus.
- `maur` **Mauryas**: units: Longbowman (100), Bamboo Spearman (100), Indian Swordsman (100), Brahmin Priest (130), Indian Light Cavalry (150), Indian Raiding Cavalry (150), Maiden Guard (190), Maiden Guard Archer (190), Warrior (220), Elephant Archer (250), War Chariot (400), Indian War Elephant (550). Heroes: Ashoka the Great, Bindusara, Acharya Chanakya, Chandragupta Maurya.
- `pers` **Persians**: units: Cardaces Hoplite (60), Cardaces Skirmisher (60), Sogdian Archer (100), Lydian Auxiliary (100), Shield Bearer (100), Persian Apple Bearer (130), Persian Immortal (130), Median Magus (130), Parthian Horse Archer (150), Hyrcanian Cavalry (150), Median Light Cavalry (150), Cappadocian Cavalry (150), Bactrian Heavy Cavalry Archer (330), Bactrian Heavy Lancer (340), Babylonian Scythed Chariot (400), Indian War Elephant (550). Heroes: Cyrus II The Great, Darius I the Great, Xerxes I.
- `ptol` **Ptolemies**: units: Cretan Mercenary Archer (60), Mercenary Thureos Skirmisher (60), Mercenary Thureos Spearman (60), Gallic Mercenary Swordsman (60), Egyptian Pikeman (100), Judean Slinger (100), Tarantine Settler Cavalry (110), Macedonian Settler Cavalry (110), Surgeon (130), Nabataean Camel Archer (150), Royal Guard Infantry (220), Royal Guard Cavalry (330), Towered War Elephant (450). Heroes: Cleopatra VII, Ptolemy I, Ptolemy IV.
- `rome` **Romans**: units: Roman Skirmisher (100), Veteran Spearman (100), Levy Auxiliary Spearman (100), Roman Swordsman (100), Slave (100), Gladiator Spearman (110), Gladiator Swordsman (110), Legionary Skirmisher (115), Marian Legionary (115), Doctor (130), Auxiliary Cavalry Javelineer (150), Allied Cavalry (150), Auxiliary Cavalry Spearman (150), Roman Cavalry (150), Italic Heavy Infantry (220), First Cohort Legionary (220), Roman Centurion (280), Consular Bodyguard (330). Heroes: Hero Cavalry Swordsman, Hero Cavalry Swordsman, Hero Cavalry Swordsman.
- `sele` **Seleucids**: units: Syrian Archer (60), Thracian Mercenary Swordsman (60), Arab Javelineer (100), Phalangite (100), Militia Thureos Spearman (100), Companion Cavalry (110), Surgeon (130), Dahae Horse Archer (150), Militia Cavalry (150), Silver Shield (220), Romanized Heavy Swordsman (220), Seleucid Cataphract (340), Scythed Chariot (400), Armored War Elephant (550). Heroes: Antiochus III the Great, Antiochus IV, Seleucus I.
- `spart` **Spartans**: units: Neodamodes Hoplite (50), Helot Skirmisher (100), Perioikoi Hoplite (100), Skiritai Commando (125), Surgeon (130), Perioikoi Cavalryman (150), Greek Allied Cavalry (150), Spartan Pikeman (220), Spartan Hoplite (220), Spartan Olympic Hoplite (220). Heroes: Agis III, Brasidas, Leonidas I.

Answer with this JSON object:

```json
{
  "civilizations": ["<id of your first choice>", "<second>", "<third>"],
  "kingdom_name": "<a name for your kingdom, max 40 characters>",
  "battle_cry": "<max 120 characters>",
  "public_message": "<optional, max 400 characters, read by the other three commanders>",
  "explanation": "<1-4 sentences for the viewers: why these choices>"
}
```
