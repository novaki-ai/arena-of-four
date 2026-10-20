# Arena of Four

Four AI models each commanded an army in the open-source strategy game **0 A.D.** (Release 28, map *Greek Acropolis*). Every order came from their answers; the game engine resolved movement and combat. This repository holds the full transcript: the rules, every question each commander received, word for word, and every answer.

**Video:** https://www.youtube.com/@Novaki_ai (link added when the video goes live)

## Commanders

| Commander | Model and setting | Civilization · kingdom name | Hero | Starting region |
|---|---|---|---|---|
| Claude (orange) | Claude Opus 5.5 · max effort · Claude Code | Carthage · The Orange Commonwealth | Hannibal | Little Hill (bid 340, paid 0) |
| GPT (green) | GPT-6 Astra · ultra · Codex | Ptolemies · The Verdant Compact | Ptolemy I | River Ford (bid 301, paid 0) |
| Gemini (blue) | Gemini 4 Argon · high (its highest) · Antigravity | Seleucids · Argon Hegemony | Seleucus I | West Plateau (bid 0) |
| Grok (white) | Grok 4.7 · xhigh · Grok Build CLI | Macedon · Pale Fire | Alexander the Great | Central Heights (bid 800, paid 800) |

Each model ran in its own official command-line tool with a clean configuration: no personal instruction files, no web search, no tools (Antigravity has no switch to turn tools off, so its prompt says not to use any and the answers were checked for tool use).

## How the match worked

1. **Kingdom**: rank three civilizations; each one can go to only one commander.
2. **Region**: rank three starting regions and bid army points for priority. You pay only if you get your first choice.
3. **Army**: spend a 4,000-point budget on units, pick a hero, organise squads.
4. **Battle**: the game pauses before every turn; all four commanders answer the same question at the same time and all orders are applied at the same moment. A ring of fire shrinks toward the centre; units outside it burn.
5. **Interview**: each commander answers a few questions about the match.

A separate orchestrator program, driven by a Claude Opus 5.5 session that did not play, sent every commander exactly the same questions and applied the answers literally. It never gave advice and never changed an answer. When an answer could not be applied as written, the warning is recorded in `warnings` and the orders actually applied are in `applied`.

The rules and question templates were locked with SHA-256 before the first question (`66067e84…`).

## Rule changes during the match

Both were requested by the match host, applied to all four commanders equally and announced inside every later question:

- **From game time 300 s (turn 13):** quiet turns are fast-forwarded. If nobody lost health and nobody declared anyone an enemy during a turn, the game keeps running in 25 s steps without a new question, up to 75 s per question. Turn 13 had already been sent to GPT and Gemini with the old text; those answers were not used and are kept in `match/discarded/turn-13-old-prompt/`. New lock `ba6a042e…`.
- **From game time 525 s (turn 18):** a turn lasts 50 s of game time instead of 25 s (quiet stretches fast-forwarded up to 100 s). New lock `d8cf7efd…`.

The match ended at 800 s of game time, as the rules say.

## Result

| Commander | At the end | Army value |
|---|---|---|
| **Claude** | Hannibal (77% health), an Italian cavalryman, an Iberian skirmisher and three Samnite spearmen | **634** |
| Grok | Alexander alone (75% health) | 374 |
| GPT | eliminated in the last turn (Hannibal killed Ptolemy) | 0 |
| Gemini | eliminated in turn 22 | 0 |

Army value = unit costs scaled by remaining health; a living hero counts 500 scaled by health. Two notes, both of which leave the winner unchanged:

- The orchestrator's own count during the match gave Claude 465. Units promoted to Elite rank change their template name, and the count did not find them in the catalog, so it scored four of Claude's units as 0. Corrected: 634.
- The rules say "a living hero counts 500". The orchestrator scaled the hero by health; several commanders read it as a flat 500 (then Claude 748, Grok 500). Claude wins under every reading.

## Files

```
rules/              the rules (sent with every question) and the five question templates, word for word
match/<stage>/      <model>.question.md     the exact question that model received
                    <model>.answer.json     attempts (raw text, seconds, token usage), parsed answer, applied answer, warnings
                    orchestrator.applied.json   the commands the orchestrator sent to the game
match/discarded/    answers to questions that were replaced by a rule change (not used)
```

Stages: `1-kingdom`, `2-region`, `3-army`, `4-turn-01` … `4-turn-23`, `5-interview`. Commanders eliminated earlier receive no further questions.

## License

The rules, questions and this README: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), Novaki. Model answers are reproduced unedited as a record of the match.

Made by [Novaki](https://www.youtube.com/@Novaki_ai) · https://novaki.lol
