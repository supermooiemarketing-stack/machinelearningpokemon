# Training settings: Qwen3.5-2B LoRA on Gen 1 OU decisions

Fine-tune `unsloth/Qwen3.5-2B` with LoRA in Unsloth Studio, on an RTX 5070 Laptop GPU (8 GB).
The data is in `data/`; see [README.md](README.md) for how it was built and checked.

## Data to load

| File | Rows | Use |
|---|---|---|
| `data/train.jsonl` | 50,000 | training |
| `data/val.jsonl` | 2,000 | evaluation during training (a 500-row prefix is enough) |
| `data/test.jsonl` | 2,000 | final offline score only; never train or tune on it |

Each row is a `messages` chat with system, user and assistant turns, the same shape as the smoke-test file.
The assistant answer is only `{"action_id":"A<n>"}`.
**Every file is shuffled, so the first N rows are a random subset.** To train on less, take a prefix of `train.jsonl`.
Splits are battle-disjoint at any prefix length.

## Settings

| Setting | Value | Why |
|---|---|---|
| Base model | `unsloth/Qwen3.5-2B` | as in the smoke test |
| Method | LoRA, 16-bit (not 4-bit) | the smoke test ran this way on the 8 GB card |
| LoRA rank / alpha / dropout | 16 / 16 / 0 | smoke-test values; raise rank to 32 only if the model underfits |
| Target modules | all attention and MLP projections (q, k, v, o, gate, up, down) | smoke-test default |
| **Max training rows** | **at least the number of rows you train on** | the smoke-test project had `max_train_rows: 1024`, which would cut the 50k set to 1,024 rows |
| Max sequence length | 1024 | longest example is 690 tokens with Qwen's chat template (median 583) |
| Loss on | **assistant answers only** ("train on completions/responses") | otherwise most of the loss is spent learning to reproduce the battle description |
| Epochs | 1 | 50k distinct decisions; repeat only if val accuracy is still rising |
| Learning rate | 1e-4, linear decay, ~3% warmup | smoke-test value |
| Batch | per-device 4, gradient accumulation 4 (effective 16) | see speed note below; drop per-device batch to 2 on out-of-memory |
| Seed | 3407 | smoke-test value |
| Evaluation | every ~250 steps on a 500-row val prefix | watch val loss and action accuracy |

### Speed and how many rows to use

The smoke test ran at batch size 1 and took about 4.5 seconds per training step (checkpoints 5→20: 67 s for 15 steps).
At that rate 1,000 rows take about 75 minutes and 10,000 rows about 12 hours.
That run left most of the GPU idle: sequences are short and the batch was 1.
Before a long run, train about 200 steps with the batch settings above and read the seconds per step.
Then choose a prefix of `train.jsonl` that fits your time budget.
A first serious run of 10,000-20,000 rows is a reasonable target; the full 50,000 if time allows.

## Using the model in the bot

- Build the prompt with the same formatter and system message as the data.
- Render the chat template with **thinking disabled** (`enable_thinking=False`). In training the template puts an empty `<think></think>` block before every answer, so inference must match.
- Decode greedily (temperature 0), `max_new_tokens` about 16.
- Parse `A<n>` from the JSON. If it is missing or not a legal action, fall back to a legal action and log the miss.
- Skip the model when Showdown offers only one option (Recharge, a forced single switch). The data contains no such turns.

## What to measure

Score each model on `data/test.jsonl`. Count the share of answers that are valid JSON with a legal ID, and the share that exactly match the human player's action.
Test-set baselines to beat:

| Baseline | Exact match |
|---|---|
| Random legal action | 14.9% |
| Always the first listed action | 20.9% |

The test set has 7.4 legal options per decision on average. Exact match against one human's choice understates play strength, because several moves are often equally good.
The real test is win rate in battles against the base model and against people.
