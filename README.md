# Machine learning Pokémon: Gen 1 OU training data

Supervised fine-tuning data for a small language model that picks one action per turn in Pokémon Showdown Gen 1 OU.
Training settings are in [TRAINING.md](TRAINING.md).

## Contents

| Path | What |
|---|---|
| `data/train.jsonl`, `val.jsonl`, `test.jsonl` | 50,000 / 2,000 / 2,000 chat examples (the model input) |
| `data/provenance/<split>.jsonl` | line-aligned source of each example: battle ID, Showdown turn, point of view, rating, source file, replay link (not model input) |
| `data/build_report.json` | build settings, counts and skipped-turn reasons |
| `data/validation_report.json` | per-example checks and token lengths |
| `data/logcheck_report.json` | label check against the original Showdown replay logs (150 random battles) |
| `review/examples.md` | ten varied examples shown next to their source turn and the replay log |

## Example format

Each line is a `{"messages": [...]}` conversation:
- **System:** a fixed instruction to pick one legal action ID and answer only with JSON.
- **User:** what the player can see this turn: their active Pokémon and team, the opponent's active Pokémon and its revealed moves, the last moves, and how many Pokémon each side has left. Then the legal actions, numbered in Metamon's order: `A0`–`A3` are the active Pokémon's moves alphabetically, `A4`–`A8` are switches alphabetically. Each move shows type, power, accuracy, PP and type effectiveness against the opponent's active Pokémon (Metamon's Gen 1 type chart).
- **Assistant:** `{"action_id":"A<n>"}`, where n is the Metamon action index the human player chose.

## How it was built

**Source:** [Metamon](https://github.com/UT-Austin-RPL/metamon) parsed replays `gen1ou.tar.gz` (revision v6).
That file has 199,024 point-of-view files from 97,139 distinct battles.

**Players imitated:** rated ladder players at 1400 or higher (the top 15% of rated players), plus all Smogon tournament games.
All tournament games are "Unrated" in the file names, so a plain numeric cutoff would have removed them.

**Splits:** battles are assigned to a split by a seeded hash of the canonical battle ID.
Both points of view of a battle stay in the same split. 1,104 battles stored twice under different ID prefixes are kept only once.

**Turn sampling:** up to 3 random turns per player per battle, drawn from the whole battle.
So the 50,000 training rows come from 9,072 battles.

**Turns left out:**
- unrevealed actions;
- turns with only one legal option;
- move labels whose move never actually ran, i.e. the next state shows neither a PP drop nor the move as last move. Metamon fills Recharge, sleep, freeze, full-paralysis and knocked-out-before-moving turns with guessed labels; compared with the Showdown logs, those labels never matched.

## Checks

- **Every example:**
  - the target is among the listed legal actions;
  - it equals Metamon's stored action;
  - it names the same move or Pokémon as Metamon's alphabetical order on the raw file;
  - re-rendering the raw state gives the identical prompt;
  - no battle and no prompt appears in two splits.
- **Length:** the longest example is 690 tokens with Qwen3.5-2B's chat template.
- **Replay logs:** in 150 random battles (810 examples), 800 labels match what the player did in that turn and 0 contradict it. The other 10 could not be compared: a mirror match where both sides had the same team, or no visible choice.

## Licence

The data is derived from Metamon's parsed replays by Jake Grigsby et al. ([jakegrigsby/metamon-parsed-replays](https://huggingface.co/datasets/jakegrigsby/metamon-parsed-replays)), licensed [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).
These files are licensed under the same terms: **non-commercial use only, with attribution**.
Changes from the source:
- states rendered as text prompts;
- turns filtered and sampled as described above;
- battle splits added.

Player names are Metamon's anonymized names.
